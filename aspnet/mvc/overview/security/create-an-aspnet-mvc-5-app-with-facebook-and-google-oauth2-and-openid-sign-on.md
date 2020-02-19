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
# <a name="create-an-aspnet-mvc-5-app-with-facebook-twitter-linkedin-and-google-oauth2-sign-on-c"></a>Crear una aplicación de ASP.NET MVC 5 con el inicio de sesión OAuth2 de Facebook, Twitter, LinkedIn y Google (C#)

por [Rick Anderson](https://twitter.com/RickAndMSFT)

> En este tutorial se muestra cómo crear una aplicación web MVC 5 de ASP.NET que permite a los usuarios iniciar sesión con [OAuth 2,0](http://oauth.net/2/) con las credenciales de un proveedor de autenticación externo, como Facebook, Twitter, LinkedIn, Microsoft o Google. Para simplificar, este tutorial se centra en el uso de credenciales de Facebook y Google.
> 
> Habilitar estas credenciales en los sitios web proporciona una ventaja significativa porque millones de usuarios ya tienen cuentas con estos proveedores externos. Estos usuarios pueden estar más inclinados para registrarse en su sitio si no tienen que crear y recordar un nuevo conjunto de credenciales.
> 
> Consulte también [aplicación ASP.NET MVC 5 con SMS y autenticación en dos fases de correo electrónico](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication.md).
> 
> En el tutorial también se muestra cómo agregar datos de perfil para el usuario y cómo usar la API de pertenencia para agregar roles. Este tutorial fue escrito por [Rick Anderson](https://blogs.msdn.com/rickAndy) (síganos en Twitter: [@RickAndMSFT](https://twitter.com/RickAndMSFT) ).

<a id="start"></a>
## <a name="getting-started"></a>Introducción

Empiece por instalar y ejecutar [Visual Studio Express 2013 para web](https://go.microsoft.com/fwlink/?LinkId=299058) o [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566). Instale Visual Studio [2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390521) o posterior. Para obtener ayuda con Dropbox, GitHub, LinkedIn, Instagram, buffer, Salesforce, vapor, Stack Exchange, TripIt, Twitch, Twitter, Yahoo!, etc., vea este [proyecto de ejemplo](https://github.com/matthewdunsdon/oauthforaspnet).

> [!NOTE]
> Debe instalar Visual Studio [2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390521) o superior para usar Google OAuth 2 y para depurar localmente sin advertencias de SSL.

Haga clic en **nuevo proyecto** en la página de **Inicio** , o bien puede usar el menú, seleccionar **archivo**y, a continuación, **nuevo proyecto**.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image1.png)  

<a id="1st"></a>
## <a name="creating-your-first-application"></a>Crear la primera aplicación

Haga clic en **nuevo proyecto**, **Seleccione C# visual** a la izquierda y luego **Web** y, a continuación, seleccione **aplicación Web de ASP.net**. Asigne al proyecto el nombre "MvcAuth" y, a continuación, haga clic en **Aceptar**.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image2.png)

En el cuadro de diálogo **nuevo proyecto de ASP.net** , haga clic en **MVC**. Si la autenticación no es una **cuenta de usuario individual**, haga clic en el botón **cambiar autenticación** y seleccione **cuentas de usuario individuales**. Al comprobar **el host en la nube**, la aplicación será muy fácil de hospedar en Azure.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image3.png)

Si seleccionó **hospedar en la nube**, complete el cuadro de diálogo Configurar.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image4.png)

### <a name="use-nuget-to-update-to-the-latest-owin-middleware"></a>Uso de NuGet para actualizar al middleware de OWIN más reciente

Use el administrador de paquetes NuGet para actualizar el [middleware OWIN](../../../aspnet/overview/owin-and-katana/getting-started-with-owin-and-katana.md). Seleccione **actualizaciones** en el menú de la izquierda. Puede hacer clic en el botón **Actualizar todo** o puede buscar solo paquetes OWIN (mostrados en la siguiente imagen):

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image5.png)

En la imagen siguiente, solo se muestran los paquetes OWIN:

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image6.png)

En la consola del administrador de paquetes (PMC), puede escribir el comando `Update-Package`, que actualizará todos los paquetes.

Presione **F5** o **Ctrl + F5** para ejecutar la aplicación. En la imagen siguiente, el número de puerto es 1234. Al ejecutar la aplicación, verá un número de puerto diferente.

En función del tamaño de la ventana del explorador, es posible que tenga que hacer clic en el icono de navegación para ver los vínculos **Inicio**, **acerca**de, **contacto**, **registro** e inicio **de sesión** .

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image7.png)  
![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image8.png) 

<a id="ssl"></a>
## <a name="setting-up-ssl-in-the-project"></a>Configuración de SSL en el proyecto

Para conectarse a proveedores de autenticación como Google y Facebook, tendrá que configurar IIS-Express para usar SSL. Es importante seguir usando SSL después del inicio de sesión y no volver a HTTP, la cookie de inicio de sesión es tan secreta como el nombre de usuario y la contraseña, y sin usar SSL, se envía en texto sin cifrar a través de la conexión. Además, ya ha tomado el tiempo necesario para realizar el protocolo de enlace y proteger el canal (que es la mayor parte de lo que hace que HTTPS sea más lento que HTTP) antes de que se ejecute la canalización de MVC, por lo que si redirige de nuevo a HTTP después de haber iniciado sesión no se realizará la solicitud actual ni el futuro las solicitudes son mucho más rápidas.

1. En **Explorador de soluciones**, haga clic en el proyecto **MvcAuth** .
2. Presione la tecla F4 para mostrar las propiedades del proyecto. Como alternativa, en el menú **Ver** puede seleccionar **ventana Propiedades**.
3. Cambie **SSL habilitado** a true.  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image9.png)
4. Copie la dirección URL de SSL (que se `https://localhost:44300/` a menos que haya creado otros proyectos SSL).
5. En **Explorador de soluciones**, haga clic con el botón derecho en el proyecto **MvcAuth** y seleccione **propiedades**.
6. Seleccione la pestaña **Web** y, a continuación, pegue la dirección URL de SSL en el cuadro **dirección URL del proyecto** . Guarde el archivo (CTL + S). Necesitará esta dirección URL para configurar las aplicaciones de autenticación de Facebook y Google.  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image10.png)
7. Agregue el atributo [RequireHttps](https://msdn.microsoft.com/library/system.web.mvc.requirehttpsattribute.aspx) al controlador de `Home` para requerir que todas las solicitudes deban usar https. Un enfoque más seguro consiste en agregar el filtro [RequireHttps](https://msdn.microsoft.com/library/system.web.mvc.requirehttpsattribute.aspx) a la aplicación. Vea la sección &quot;protección de la aplicación con SSL y el atributo Authorize&quot; en mi tutorial [creación de una aplicación de ASP.NET MVC con autenticación y base de SQL Database e implementación en Azure App Service](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data). A continuación se muestra una parte del controlador Home.

    [!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample1.cs?highlight=1)]
8. Presione CTRL+F5 para ejecutar la aplicación. Si ha instalado el certificado en el pasado, puede omitir el resto de esta sección y pasar a [crear una aplicación de Google para OAuth 2 y conectar la aplicación al proyecto](#goog); de lo contrario, siga las instrucciones para confiar en el certificado autofirmado que ha generado IIS Express.  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image11.png)
9. Lea el cuadro de diálogo **Advertencia de seguridad** y, a continuación, haga clic en **sí** si desea instalar el certificado que representa localhost.  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image12.png)
10. IE muestra la página *Inicio* , sin ninguna advertencia de SSL.  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image13.png)
11. Google Chrome también acepta el certificado y mostrará el contenido HTTPS sin ninguna advertencia. Firefox usa su propio almacén de certificados, por lo que mostrará una advertencia. Para nuestra aplicación, puede hacer clic en entiendo **los riesgos**.   
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image14.png)

<a id="goog"></a>
## <a name="creating-a-google-app-for-oauth-2-and-connecting-the-app-to-the-project"></a>Crear una aplicación de Google para OAuth 2 y conectar la aplicación al proyecto

> [!WARNING]
> Para obtener instrucciones actuales de Google OAuth, consulte [configuración de la autenticación de Google en ASP.net Core](/aspnet/core/security/authentication/social/google-logins).

1. Navegue a la [consola de desarrolladores de Google](https://console.developers.google.com/).
2. Si no ha creado un proyecto antes, seleccione **credenciales** en la pestaña de la izquierda y, a continuación, seleccione **crear**.
3. En la pestaña de la izquierda, haga clic en **credenciales**.
4. Haga clic en **crear credenciales** y luego en ID. de **cliente OAuth**. 

    1. En el cuadro de diálogo **crear ID** . de cliente, mantenga la **aplicación web** predeterminada para el tipo de aplicación.
    2. Establezca los orígenes de **JavaScript autorizados** en la dirección URL de SSL que usó anteriormente (`https://localhost:44300/` a menos que haya creado otros proyectos SSL)
    3. Establezca el **URI de redireccionamiento autorizado** en:  
         `https://localhost:44300/signin-google`
5. Haga clic en el elemento de menú pantalla de consentimiento de OAuth y, a continuación, establezca la dirección de correo electrónico y el nombre del producto. Cuando haya completado el formulario, haga clic en **Guardar**.
6. Haga clic en el elemento de menú biblioteca, busque **Google + API**, haga clic en él y, a continuación, presione habilitar.
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image15.png)  
  
   En la imagen siguiente se muestran las API habilitadas.  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image16.png)
7. En el administrador de API de API de Google, visite la pestaña **credenciales** para obtener el **identificador de cliente**. Descargar para guardar un archivo JSON con secretos de aplicación. Copie y pegue los **ClientID** y **ClientSecret** en el método `UseGoogleAuthentication` que se encuentra en el archivo *Startup.auth.CS* de la carpeta *App_Start* . Los valores de **ClientID** y **ClientSecret** que se muestran a continuación son ejemplos y no funcionan.

    [!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample2.cs?highlight=37-39)]

    > [!WARNING]
    > Seguridad: nunca almacene datos confidenciales en el código fuente. La cuenta y las credenciales se agregan al código anterior para mantener el ejemplo simple. Vea los [procedimientos recomendados para implementar contraseñas y otros datos confidenciales en ASP.net y Azure App Service](../../../identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md).
8. Presione **CTRL+F5** para compilar y ejecutar la aplicación. Haga clic en el vínculo **Iniciar sesión** .  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image17.png)
9. En **usar otro servicio para iniciar sesión**, haga clic en **Google**.  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image18.png)

    > [!NOTE]
    > Si se pierde alguno de los pasos anteriores, obtendrá un error HTTP 401. Volver a comprobar los pasos anteriores. Si se pierde una configuración necesaria (por ejemplo, el **nombre del producto**), agregue el elemento que falta y guárdelo. la autenticación puede tardar unos minutos en funcionar.
10. Se le redirigirá al sitio de Google en el que escribirá sus credenciales.   
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image19.png)
11. Una vez que haya proporcionado sus credenciales, el sistema le pedirá que le dé permisos a la aplicación web que acaba de crear:
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image20.png)
12. Haga clic en **Aceptar**. Ahora se le redirigirá a la página de **registro** de la aplicación MvcAuth donde puede registrar su cuenta de Google. Tiene la opción de cambiar el nombre de registro de correo electrónico local que usa para su cuenta de Gmail, pero suele ser más práctico mantener el mismo alias de correo electrónico predeterminado (es decir, el que usó para la autenticación). Haga clic en **Registrar**.  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image21.png)

<a id="fb"></a>
## <a name="creating-the-app-in-facebook-and-connecting-the-app-to-the-project"></a>Crear la aplicación en Facebook y conectar la aplicación al proyecto

> [!WARNING]
> Para obtener instrucciones de autenticación de OAuth2 de Facebook actuales, consulte Configuración de la [autenticación de Facebook](/aspnet/core/security/authentication/social/facebook-logins) .

<a id="mdb"></a>
## <a name="examine-the-membership-data"></a>Examinar los datos de pertenencia

En el menú **Ver** , haga clic en **Explorador de servidores**.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image32.png)

Expanda **DefaultConnection (MvcAuth)** , expanda **tablas**, haga clic con el botón secundario en **AspNetUsers** y haga clic en **Mostrar datos de tabla**.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image33.png)

![datos de la tabla aspnetusers](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image34.png)

<a id="ap"></a>
## <a name="adding-profile-data-to-the-user-class"></a>Agregar datos de perfil a la clase User

En esta sección, agregará la fecha de nacimiento y la ciudad de inicio a los datos de usuario durante el registro, como se muestra en la siguiente imagen.

![REG con ciudad de casa y Bday](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image35.png)

Abra el archivo *Models\IdentityModels.CS* y agregue las propiedades fecha de nacimiento y ciudad de residencia:

[!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample4.cs?highlight=3-4)]

Abra el archivo *Models\AccountViewModels.CS* y las propiedades establecer fecha de nacimiento y ciudad de casa en `ExternalLoginConfirmationViewModel`.

[!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample5.cs?highlight=8-9)]

Abra el archivo *Controllers\AccountController.CS* y agregue el código para la fecha de nacimiento y la ciudad de inicio en el método de acción `ExternalLoginConfirmation`, como se muestra a continuación:

[!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample6.cs?highlight=21-23)]

Agregue la fecha de nacimiento y la ciudad de inicio al archivo *Views\Account\ExternalLoginConfirmation.cshtml* :

[!code-cshtml[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample7.cshtml?highlight=27-40)]

Elimine la base de datos de pertenencia para que vuelva a registrar su cuenta de Facebook en la aplicación y compruebe que puede Agregar la nueva fecha de nacimiento y la información del perfil de ciudad de casa.

En **Explorador de soluciones**, haga clic en el icono **Mostrar todos los archivos** y, a continuación, haga clic con el botón secundario en *agregar\_Data\aspnet-MvcAuth-&lt;fecha&gt;. MDF* y haga clic en **eliminar**.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image36.png)

En el menú **herramientas** , haga clic en Administrador de **paquetes NuGet**y, a continuación, haga clic en **consola del administrador de paquetes** (PMC). Escriba los siguientes comandos en el PMC.

1. Habilitar: migraciones
2. Agregar inicialización de migración
3. Actualizar base de datos

Ejecute la aplicación y use FaceBook y Google para iniciar sesión y registrar a algunos usuarios.

## <a name="examine-the-membership-data"></a>Examinar los datos de pertenencia

En el menú **Ver** , haga clic en **Explorador de servidores**.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image37.png)

Haga clic con el botón secundario en **AspNetUsers** y haga clic en **Mostrar datos de tabla**.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image38.png)

A continuación se muestran los campos `HomeTown` y `BirthDate`.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image39.png)

<a id="off"></a>
## <a name="logging-off-your-app-and-logging-in-with-another-account"></a>Cierre la sesión de la aplicación e inicie sesión con otra cuenta

Si inicia sesión en su aplicación con Facebook, y, a continuación, cierre la sesión e intente iniciar sesión de nuevo con una cuenta de Facebook diferente (con el mismo explorador), iniciará sesión inmediatamente en la cuenta de Facebook anterior que usó. Para usar otra cuenta, debe navegar a Facebook y cerrar sesión en Facebook. La misma regla se aplica a cualquier otro proveedor de autenticación de terceros. Como alternativa, puede iniciar sesión con otra cuenta mediante un explorador diferente.

## <a name="next-steps"></a>Pasos siguientes

Consulte [Introducción a los proveedores de seguridad de Yahoo y LinkedIn OAuth para OWIN](http://www.jerriepelser.com/blog/introducing-the-yahoo-linkedin-oauth-security-providers-for-owin/) by Jerrie Pelser para las instrucciones de Yahoo y LinkedIn. Consulte botones de inicio de sesión social Pretty de Jerrie para ASP.NET MVC 5 para obtener los botones Habilitar inicio de sesión social.

Siga mi tutorial [creación de una aplicación de ASP.NET MVC con auth y SQL Database e impleméntela en Azure App Service](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data), que continúa en este tutorial y muestra lo siguiente:

1. Cómo implementar la aplicación en Azure.
2. Cómo proteger la aplicación con roles.
3. Cómo proteger la aplicación con los filtros [RequireHttps](https://msdn.microsoft.com/library/system.web.mvc.requirehttpsattribute(v=vs.108).aspx) y [Authorize](https://msdn.microsoft.com/library/system.web.mvc.authorizeattribute(v=vs.100).aspx) .
4. Cómo usar la API de pertenencia para agregar usuarios y roles.

Deje comentarios sobre cómo le gustó este tutorial y lo que podríamos mejorar. También puede solicitar nuevos temas en [mostrarme cómo con el código](http://aspnet.uservoice.com/forums/228522-show-me-how-with-code). Incluso puede solicitar y votar las nuevas características que se van a agregar a ASP.NET. Por ejemplo, puede votar por una herramienta para [crear y administrar usuarios y roles.](http://aspnet.uservoice.com/forums/41199-general-asp-net/suggestions/5646857-asp-net-identity-membership-db-tool-to-mangage-use)

Para obtener una buena explicación de cómo funcionan los servicios de autenticación externos de ASP.NET, consulte [servicios de autenticación externos](https://asp.net/web-api/overview/security/external-authentication-services)de Robert McMurray. El artículo de Robert también incluye detalles sobre cómo habilitar la autenticación de Microsoft y Twitter. En el excelente [tutorial de EF/MVC](../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md) de Tom Dykstra se muestra cómo trabajar con el Entity Framework.
