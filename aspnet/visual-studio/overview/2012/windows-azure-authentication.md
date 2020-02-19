---
uid: visual-studio/overview/2012/windows-azure-authentication
title: Autenticación de Windows Azure | Microsoft Docs
author: Rick-Anderson
description: Microsoft ASP.NET Tools para Windows Azure Active Directory facilita la habilitación de la autenticación para las aplicaciones web hospedadas en sitios web de Windows Azure...
ms.author: riande
ms.date: 02/20/2013
ms.assetid: a3cef801-a54b-4ebd-93c3-55764e2e14b1
msc.legacyurl: /visual-studio/overview/2012/windows-azure-authentication
msc.type: authoredcontent
ms.openlocfilehash: ce98effe18dd739504fb0d5453bae8a46c3ba102
ms.sourcegitcommit: 7709c0a091b8d55b7b33bad8849f7b66b23c3d72
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/19/2020
ms.locfileid: "77457484"
---
# <a name="windows-azure-authentication"></a>Autenticación de Windows Azure

por [Rick Anderson](https://twitter.com/RickAndMSFT)

> Microsoft ASP.NET Tools para Windows Azure Active Directory facilita la habilitación de la autenticación para las aplicaciones web hospedadas en [sitios web de Windows Azure](https://www.windowsazure.com/home/features/web-sites/). Puede usar la autenticación de Windows Azure para autenticar a los usuarios de Office 365 de su organización, las cuentas corporativas sincronizadas desde su Active Directory local o los usuarios creados en su propio dominio personalizado de Windows Azure Active Directory. Al habilitar la autenticación de Windows Azure, se configura la aplicación para autenticar a los usuarios mediante un solo inquilino de [Windows Azure Active Directory](https://docs.microsoft.com/azure/active-directory/) .
>
> La herramienta de autenticación de Windows Azure ASP.NET no se admite para los roles Web en un servicio en la nube, pero tenemos previsto hacerlo en una versión futura. [Windows Identity Foundation](https://msdn.microsoft.com/library/hh291066(v=VS.110).aspx) (WIF) se admite en los roles Web de Windows Azure.
>
> Para obtener más información sobre cómo configurar la sincronización entre el Active Directory local y el inquilino de Windows Azure Active Directory, consulte [usar AD FS 2,0 para implementar y administrar el inicio de sesión único](https://technet.microsoft.com/library/jj205462.aspx).
>
> Windows Azure Active Directory está disponible actualmente como [servicio de vista previa gratis](https://azure.microsoft.com/free/?WT.mc_id=A443DD604).

## <a name="requirements"></a>Requisitos:

- Visual Studio 2012 o [Visual Studio Express 2012](https://www.microsoft.com/visualstudio/11/products/express)
- [Extensiones de herramientas web para Visual Studio 2012](https://go.microsoft.com/fwlink/?LinkID=282228&amp;clcid=0x409) o [extensiones de herramientas web para Visual Studio Express 2012](https://go.microsoft.com/fwlink/?LinkID=282231&amp;clcid=0x409)
- [Herramientas de Microsoft ASP.net para windows Azure Active Directory: Visual Studio 2012](https://go.microsoft.com/fwlink/?LinkID=282306) o [Microsoft ASP.net tools para windows Azure Active Directory: Visual Studio Express 2012 para web](https://go.microsoft.com/fwlink/?LinkId=282652)

## <a name="create-an-aspnet-web-application-with-visual-studio-2012"></a>Creación de una aplicación Web de ASP.NET con Visual Studio 2012

Puede crear cualquier aplicación web con Visual Studio 2012, en este tutorial se usa la plantilla de intranet de ASP.NET MVC.

1. Cree una nueva aplicación de intranet ASP.NET MVC 4 y acepte todos los valores predeterminados. (Debe ser una en **tra** net y no en el proyecto **ter** net).
     ![](windows-azure-authentication/_static/image1.png)

## <a name="enable-window-azure-authentication-when-you-are-a-global-administrator-of-the-tenet"></a>Habilitación de la autenticación de Windows Azure (cuando es un administrador global del principio)

Si no tiene un inquilino de Windows Azure Active Directory existente (por ejemplo, a través de una cuenta existente de Office 365), puede crear un nuevo inquilino. para ello, Regístrese para obtener una [nueva cuenta de Windows Azure Active Directory](https://g.microsoftonline.com/0AX00en/5).

1. En el menú proyecto, seleccione **Habilitar autenticación de Windows Azure**:

   ![](windows-azure-authentication/_static/image2.png)

2. Escriba el dominio del inquilino de Windows Azure Active Directory (por ejemplo, contoso.onmicrosoft.com) y haga clic en **Habilitar**:

![](windows-azure-authentication/_static/image3.png)

3. En el cuadro de diálogo de autenticación Web, inicie sesión como administrador del inquilino de Windows Azure Active Directory:

   ![](windows-azure-authentication/_static/image4.png)

![](windows-azure-authentication/_static/image5.png)

## <a name="enable-window-azure-by-a-non-administrator-of-the-tenet"></a>Habilitación de Windows Azure por parte de un no administrador del principio

Si no tiene privilegios de administrador global para el inquilino de Windows Azure Active Directory, puede desactivar la casilla para aprovisionar la aplicación.

![](windows-azure-authentication/_static/image6.png)

El cuadro de diálogo mostrará el **dominio**, el **identificador** de la entidad de seguridad de la aplicación y la **dirección URL de respuesta** necesarios para aprovisionar la aplicación con un Azure Active Directory principio. Debe proporcionar esta información a alguien que tenga privilegios suficientes para aprovisionar la aplicación. Consulte[cómo implementar el inicio de sesión único con la aplicación Azure Active Directory-ASP.net de Windows](https://github.com/Azure-Samples/active-directory-dotnet-webapp-openidconnect) para obtener más información sobre cómo usar el cmdlet para crear la entidad de servicio manualmente.
Una vez que la aplicación se haya aprovisionado correctamente, puede hacer clic en **continuar para actualizar Web. config con la configuración seleccionada**. Si desea continuar desarrollando la aplicación mientras espera a que se produzca el aprovisionamiento, puede hacer clic en **Cerrar para recordar la configuración del archivo de proyecto**. La próxima vez que se invoque habilitar autenticación de Windows Azure y desactive la casilla aprovisionamiento, verá la misma configuración y podrá hacer clic en **continuar**y, a continuación, en **aplicar esta configuración en el archivo Web. config**.

1. Espere mientras la aplicación está configurada para la autenticación de Windows Azure y aprovisionada con Windows Azure Active Directory.
2. Una vez que se haya habilitado la autenticación de Windows Azure para su aplicación, haga clic en **Cerrar:**

    ![](windows-azure-authentication/_static/image7.png)
3. Presione F5 para ejecutar la aplicación. Debe redirigirse automáticamente a la página de inicio de sesión. Use las credenciales de usuario del principio del directorio para iniciar sesión en la aplicación.

    ![](windows-azure-authentication/_static/image1.jpg)
4. Dado que la aplicación está usando un certificado de prueba autofirmado, recibirá una advertencia del explorador que indica que el certificado no fue emitido por una entidad de certificación de confianza.

    Esta advertencia puede omitirse de forma segura durante el desarrollo local haciendo clic en **pasar a este sitio web:**

    ![](windows-azure-authentication/_static/image8.png)
5. Ha iniciado sesión correctamente en la aplicación con la autenticación de Windows Azure.

    ![](windows-azure-authentication/_static/image2.jpg)

La habilitación de la autenticación de Windows Azure realiza los siguientes cambios en la aplicación:

- Se agrega una clase de falsificación de solicitud entre sitios ([CSRF](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF))) ( *App\_Start\AntiXsrfConfig.CS* ) al proyecto.
- Los paquetes NuGet `System.IdentityModel.Tokens.ValidatingIssuerNameRegistry` se agregan al proyecto.
- La configuración de Windows Identity Foundation en la aplicación se configurará para aceptar los tokens de seguridad del inquilino de Windows Azure Active Directory. Haga clic en la imagen siguiente para ver una vista expandida de los cambios realizados en el archivo *Web. config* .

     ![](windows-azure-authentication/_static/image9.png)
- Se aprovisionará una entidad de servicio para la aplicación en el inquilino de Windows Azure Active Directory.
- HTTPS está habilitado.

## <a name="deploy-the-application-to-windows-azure"></a>Implementar la aplicación en Windows Azure

Para obtener instrucciones completas, consulte [implementar una aplicación Web de ASP.net en un sitio web de Windows Azure](https://docs.microsoft.com/azure/app-service-web/app-service-web-get-started-dotnet).

Para publicar una aplicación con la autenticación de Windows Azure en un sitio web de Azure:

1. Haga clic con el botón derecho en la aplicación y seleccione **publicar:**

    ![](windows-azure-authentication/_static/image3.jpg)
2. En el cuadro de diálogo publicar web, descargue e importe un perfil de publicación para el sitio web de Azure.

    ![](windows-azure-authentication/_static/image4.jpg)
3. La pestaña **conexión** muestra la **dirección URL de destino** (la dirección URL pública de la aplicación). Haga clic en **validar conexión** para probar la conexión:

    ![](windows-azure-authentication/_static/image5.jpg)
4. Si ha publicado en este sitio web de Azure, considere la posibilidad de comprobar la configuración de **quitar archivos adicionales en el destino** para asegurarse de que la aplicación se publica correctamente. Observe que la casilla **Habilitar autenticación de Windows Azure** está activada.

    ![](windows-azure-authentication/_static/image10.png)
5. Opcional: en la pestaña **vista previa** , haga clic en **iniciar vista previa** para ver los archivos implementados.

    ![](windows-azure-authentication/_static/image6.jpg)
6. Haga clic en **publicar.**

    Se le pedirá que habilite la autenticación de Windows Azure para el host de destino. Haga clic en **Habilitar** para continuar:

    ![](windows-azure-authentication/_static/image11.png)
7. Escriba sus credenciales de administrador para el inquilino de Windows Azure Active Directory:

    ![](windows-azure-authentication/_static/image7.jpg)
8. Una vez que la aplicación se haya publicado correctamente, se abrirá un explorador en el sitio Web publicado.

    > [!NOTE]
    > La aplicación puede tardar hasta cinco minutos (normalmente, menos) en estar aprovisionado por completo con Windows Azure Active Directory después de habilitar la autenticación de Windows Azure para el host de destino. La primera vez que ejecute la aplicación, si recibe el error ACS50001: no se encontró el usuario de confianza con el nombre ' [Realm] ', espere unos minutos e intente ejecutar la aplicación de nuevo.
9. Cuando se le solicite, inicie sesión como un usuario en su directorio:

    ![](windows-azure-authentication/_static/image8.jpg)
10. Ha iniciado sesión correctamente en la aplicación hospedada de Azure con la autenticación de Windows Azure.

     ![](windows-azure-authentication/_static/image9.jpg)

## <a name="known-issues"></a>Problemas conocidos

#### <a name="role-based-authorization-fails-when-using-windows-azure-authentication"></a>Se produce un error en la autorización basada en roles cuando se usa la autenticación de Windows Azure

La autenticación de Windows Azure no proporciona actualmente la demanda de rol necesaria para que se pueda realizar la autorización basada en roles. El rol del usuario autenticado debe recuperarse manualmente de Windows Azure Active Directory.

#### <a name="browsing-to-an-application-with-windows-azure-authentication-results-in-the-error-acs20016-the-domain-of-the-logged-in-user-livecom-does-not-match-any-allowed-domain-of-this-sts"></a>Al buscar una aplicación con la autenticación de Windows Azure se produce el error "ACS20016 el dominio del usuario que ha iniciado sesión (live.com) no coincide con ningún dominio permitido de este STS"

Si ya ha iniciado sesión en una cuenta de Microsoft (por ejemplo, hotmail.com, live.com, outlook.com) e intenta acceder a una aplicación que tiene habilitada la autenticación de Windows Azure, es posible que reciba una respuesta de error 400 porque el dominio de su cuenta de Microsoft Windows Azure Active Directory no reconoce. Para iniciar sesión en la aplicación, primero cierre la sesión de su cuenta de Microsoft.

#### <a name="logging-into-an-application-with-windows-azure-authentication-enabled-and-a-x509certificatevalidationmode-other-than-none-results-in-certificate-validation-errors-for-the-accountsaccesscontrolwindowsnet-certificate"></a>El inicio de sesión en una aplicación con la autenticación de Windows Azure habilitada y un X509CertificateValidationMode distinto de None produce errores de validación de certificados para el certificado accounts.accesscontrol.windows.net

No es necesaria la validación de certificados y debe dejarse deshabilitada. El WSFederationAuthenticationModule valida la huella digital del certificado del emisor.

#### <a name="when-attempting-to-enable-windows-azure-authentication-the-web-authentication-dialog-shows-the-error-acs20016-the-domain-of-the-logged-in-user-contosoonmicrosoftcom-does-not-match-any-allowed-domain-of-this-sts"></a>Al intentar habilitar la autenticación de Windows Azure, el cuadro de diálogo de autenticación Web muestra el error "ACS20016: el dominio del usuario que inició sesión (contoso.onmicrosoft.com) no coincide con ningún dominio permitido de este STS".

Puede ver este error si ha iniciado sesión correctamente con una cuenta de Windows Azure Active Directory diferente en el mismo proceso de Visual Studio. Cierre la sesión de la cuenta especificada o reinicie Visual Studio. Si ha iniciado sesión previamente y ha seleccionado la opción "mantener la sesión iniciada", es posible que tenga que borrar las cookies del explorador.

## <a name="acs20012-the-request-is-not-a-valid-ws-federation-protocol-message"></a>ACS20012: la solicitud no es un mensaje de protocolo WS-Federation válido

Esto puede ocurrir si ya ha iniciado sesión con algún otro identificador de Microsoft en uno de los servicios de Azure. Use la ventana privada del explorador como InPrivate en IE o incógnito en Chrome o borre todas las cookies.

## <a name="additional-resources"></a>Recursos adicionales

- [Herramientas de Microsoft ASP.net para Windows Azure Active Directory: Visual Studio 2012](https://blogs.msdn.com/b/vbertocci/archive/2013/02/18/microsoft-asp-net-tools-for-windows-azure-active-directory-visual-studio-2012.aspx) – Vittorio BERTOCCI
- [Características de Windows Azure: identidad](https://docs.microsoft.com/azure/active-directory/)
- [TechNet: Azure Active Directory de Windows](https://technet.microsoft.com/library/hh967619.aspx)
- [Windows Azure Active Directory: desarrollo de aplicaciones para su organización](https://activedirectory.windowsazure.com/Develop/Single-Tenant.aspx)
- [Windows Azure Active Directory: desarrollo de aplicaciones para varias organizaciones](https://activedirectory.windowsazure.com/Develop/Multi-Tenant.aspx)
- [Cómo implementar el inicio de sesión único con Windows Azure Active Directory](https://github.com/Azure-Samples/active-directory-dotnet-webapp-openidconnect)
- [Inicio de sesión único con Windows Azure Active Directory: un análisis profundo](https://blogs.msdn.com/b/vbertocci/archive/2012/07/05/single-sign-on-with-windows-azure-active-directory-a-deep-dive.aspx) : Vittorio BERTOCCI
- [Usar AD FS 2,0 para implementar y administrar el inicio de sesión único](https://technet.microsoft.com/library/jj205462.aspx)
