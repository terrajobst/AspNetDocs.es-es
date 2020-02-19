---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/single-sign-on
title: Inicio de sesión único (creación de aplicaciones en la nube reales con Azure) | Microsoft Docs
author: MikeWasson
description: La creación de aplicaciones reales en la nube con el libro electrónico de Azure se basa en una presentación desarrollada por Scott Guthrie. Se explican 13 patrones y prácticas que pueden...
ms.author: riande
ms.date: 06/12/2014
ms.assetid: 7d82d5e9-0619-4f22-9e03-32a6d52940a5
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/single-sign-on
msc.type: authoredcontent
ms.openlocfilehash: 1ca93cce22487295a24aae95437b3e69dfc5b504
ms.sourcegitcommit: 7709c0a091b8d55b7b33bad8849f7b66b23c3d72
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/19/2020
ms.locfileid: "77457146"
---
# <a name="single-sign-on-building-real-world-cloud-apps-with-azure"></a>Inicio de sesión único (creación de aplicaciones en la nube reales con Azure)

por [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson](https://twitter.com/RickAndMSFT), [Tom Dykstra](https://github.com/tdykstra)

[Descargar el proyecto de corrección de ti](https://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) o [descargar el libro electrónico](https://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> La **creación de aplicaciones reales en la nube con** el libro electrónico de Azure se basa en una presentación desarrollada por Scott Guthrie. Se explican 13 patrones y prácticas que pueden ayudarle a desarrollar correctamente aplicaciones web para la nube. Para obtener información sobre el libro electrónico, consulte [el primer capítulo](introduction.md).

Hay muchos problemas de seguridad que se deben tener en cuenta al desarrollar una aplicación en la nube, pero para esta serie nos centraremos solo en uno: Inicio de sesión único. Una pregunta que a menudo se pregunta es: "soy principalmente la creación de aplicaciones para los empleados de mi empresa; ¿Cómo puedo hospedar estas aplicaciones en la nube y seguir pudiendo usar el mismo modelo de seguridad que mis empleados saben y usan en el entorno local cuando ejecutan aplicaciones que se hospedan dentro del firewall? " Una de las formas en que se habilita este escenario se denomina Azure Active Directory (Azure AD). Azure AD le permite hacer que las aplicaciones de línea de negocio (LOB) de la empresa estén disponibles a través de Internet y permite poner estas aplicaciones a disposición de los socios comerciales.

## <a name="introduction-to-azure-ad"></a>Introducción a Azure AD

[Azure ad](https://docs.microsoft.com/azure/active-directory/) proporciona [Active Directory](https://msdn.microsoft.com/library/windows/desktop/aa746492.aspx) en la nube. Entre las principales características se incluyen las siguientes:

- Se integra con Active Directory local.
- Habilita el inicio de sesión único con sus aplicaciones.
- Admite estándares abiertos como [SAML](http://en.wikipedia.org/wiki/SAML_2.0), [WS-Fed](http://en.wikipedia.org/wiki/WS-Federation)y [OAuth 2,0](http://oauth.net/2/).
- Es compatible con la [API de REST](https://msdn.microsoft.com/library/hh974476.aspx)de Enterprise Graph.

Supongamos que tiene un entorno de Active Directory de Windows Server local que se usa para permitir que los empleados inicien sesión en las aplicaciones de la intranet:

![](single-sign-on/_static/image1.png)

Lo que Azure AD le permite hacer es crear un directorio en la nube. Es una característica gratuita y fácil de configurar.

Puede ser completamente independiente de la Active Directory local; puede colocar a cualquiera que desee en él y autenticarlos en aplicaciones de Internet.

![Microsoft Azure Active Directory](single-sign-on/_static/image2.png)

También puede integrarlo con su instancia de AD local.

![Integración de AD y WAAD](single-sign-on/_static/image3.png)

Ahora todos los empleados que pueden autenticarse en el entorno local también pueden autenticarse a través de Internet, sin tener que abrir un firewall o implementar nuevos servidores en el centro de datos. Puede seguir aprovechando todo el entorno de Active Directory existente que ya conoce y que usa hoy mismo para ofrecer a la capacidad de inicio de sesión único de las aplicaciones internas.

Una vez que haya realizado esta conexión entre AD y Azure AD, también puede habilitar las aplicaciones web y los dispositivos móviles para autenticar a sus empleados en la nube, y puede habilitar aplicaciones de terceros, como Office 365, SalesForce.com o Google Apps, para aceptar su credenciales de los empleados. Si usa Office 365, ya está configurado con Azure AD porque Office 365 usa Azure AD para la autenticación y la autorización.

![aplicaciones de terceros](single-sign-on/_static/image4.png)

La belleza de este enfoque es que cada vez que la organización agrega o elimina un usuario, o un usuario cambia una contraseña, usa el mismo proceso que usa hoy en su entorno local. Todos los cambios de AD locales se propagan automáticamente al entorno de nube.

Si su empresa usa o traslada a Office 365, la buena noticia es que tendrá Azure AD configurado automáticamente porque Office 365 usa Azure AD para la autenticación. Para que pueda usar fácilmente en sus propias aplicaciones la misma autenticación que usa Office 365.

## <a name="set-up-an-azure-ad-tenant"></a>Configuración de un inquilino de Azure AD

un directorio de Azure AD se denomina [inquilino](https://technet.microsoft.com/library/jj573650.aspx)de Azure ad y la configuración de un inquilino es bastante fácil. Le mostraremos cómo se lleva a cabo en Azure Portal de administración para ilustrar los conceptos, pero, por supuesto, como las otras funciones del portal, también puede hacerlo mediante un script o una API de administración.

En el portal de administración, haga clic en la pestaña Active Directory.

![WAAD en el portal](single-sign-on/_static/image5.png)

Tiene automáticamente un inquilino Azure AD para su cuenta de Azure y puede hacer clic en el botón **Agregar** situado en la parte inferior de la página para crear directorios adicionales. Es posible que desee una para un entorno de prueba y otra para producción, por ejemplo. Piense detenidamente en el nombre de un nuevo directorio. Si usa el nombre del directorio y, a continuación, usa de nuevo el nombre de uno de los usuarios, puede resultar confuso.

![Agregar directorio](single-sign-on/_static/image6.png)

El portal es totalmente compatible con la creación, eliminación y administración de usuarios dentro de este entorno. Por ejemplo, para agregar un usuario, vaya a la pestaña **usuarios** y haga clic en el botón **Agregar usuario** .

![Botón Agregar usuario](single-sign-on/_static/image7.png)

![Cuadro de diálogo Agregar usuario](single-sign-on/_static/image8.png)

Puede crear un nuevo usuario que solo exista en este directorio, o puede registrar una cuenta Microsoft como usuario en este directorio, o bien registrarse o un usuario de otro Azure AD directorio como usuario en este directorio. (En un directorio real, el dominio predeterminado sería ContosoTest.onmicrosoft.com. También puede usar un dominio de su elección, como contoso.com).

![Tipos de usuario](single-sign-on/_static/image9.png)

![Cuadro de diálogo Agregar usuario](single-sign-on/_static/image10.png)

Puede asignar el usuario a un rol.

![Perfil de usuario](single-sign-on/_static/image11.png)

Y la cuenta se crea con una contraseña temporal.

![Contraseña temporal](single-sign-on/_static/image12.png)

Los usuarios que cree de esta manera pueden iniciar sesión inmediatamente en sus aplicaciones web con este directorio en la nube.

Sin embargo, lo ideal para el inicio de sesión único empresarial es la pestaña **integración de directorios** :

![Pestaña integración de directorios](single-sign-on/_static/image13.png)

Si habilita la integración de directorios y [descarga una herramienta](https://social.technet.microsoft.com/wiki/contents/articles/19098.howto-install-the-windows-azure-active-directory-sync-tool-now-with-pictures.aspx), puede sincronizar este directorio en la nube con la Active Directory local existente que ya usa dentro de su organización. Todos los usuarios almacenados en el directorio se mostrarán en este directorio en la nube. Las aplicaciones en la nube ahora pueden autenticar a todos los empleados con sus credenciales de Active Directory existentes. Y todo esto es gratuito: la herramienta de sincronización y la propia Azure AD.

La herramienta es un asistente que es fácil de usar, como puede ver en estas capturas de pantalla. Estas no son instrucciones completas, solo un ejemplo en el que se muestra el proceso básico. Para obtener información más detallada sobre cómo hacerlo, consulte los vínculos en la sección de [recursos](#resources) al final del capítulo.

![Asistente para configuración de la herramienta de sincronización de WAAD](single-sign-on/_static/image14.png)

Haga clic en **siguiente**y, a continuación, escriba sus credenciales de Azure Active Directory.

![Asistente para configuración de la herramienta de sincronización de WAAD](single-sign-on/_static/image15.png)

Haga clic en **siguiente**y, a continuación, escriba sus credenciales de ad locales.

![Asistente para configuración de la herramienta de sincronización de WAAD](single-sign-on/_static/image16.png)

Haga clic en **siguiente**y, a continuación, indique si desea almacenar un hash de las contraseñas de AD en la nube.

![Asistente para configuración de la herramienta de sincronización de WAAD](single-sign-on/_static/image17.png)

El hash de contraseña que puede almacenar en la nube es un hash unidireccional. las contraseñas reales nunca se almacenan en Azure AD. Si decide almacenar los valores hash en la nube, tendrá que usar [servicios de Federación de Active Directory (AD FS)](https://technet.microsoft.com/library/hh831502.aspx) (ADFS). También hay [otros factores que deben tenerse en cuenta a la hora de elegir si quiere usar ADFS o no](https://technet.microsoft.com/library/jj573653.aspx). La opción ADFS requiere algunos pasos de configuración adicionales.

Si decide almacenar los valores hash en la nube, habrá terminado y la herramienta iniciará la sincronización de directorios al hacer clic en **siguiente**.

![Asistente para configuración de la herramienta de sincronización de WAAD](single-sign-on/_static/image18.png)

Y en unos minutos habrá terminado.

![Asistente para configuración de la herramienta de sincronización de WAAD](single-sign-on/_static/image19.png)

Solo tiene que ejecutarlo en un controlador de dominio de la organización, en Windows 2003 o posterior. Y no es necesario reiniciar. Cuando haya terminado, todos los usuarios están en la nube y puede realizar el inicio de sesión único desde cualquier aplicación web o móvil, mediante SAML, OAuth o WS-FED.

A veces le preguntamos cómo es seguro esto: ¿usa Microsoft para sus datos empresariales confidenciales? Y la respuesta es sí. Por ejemplo, si va al sitio interno de Microsoft SharePoint en [https://microsoft.sharepoint.com/](https://microsoft.sharepoint.com/), se le pedirá que inicie sesión.

![Inicio de sesión en Office 365](single-sign-on/_static/image20.png)

Microsoft ha habilitado ADFS, por lo que, cuando escriba un identificador de Microsoft, se le redirigirá a una página de inicio de sesión de ADFS.

![Inicio de sesión de ADFS](single-sign-on/_static/image21.png)

Y, una vez que escriba las credenciales almacenadas en una cuenta de Microsoft AD interna, tendrá acceso a esta aplicación interna.

![Sitio de MS SharePoint](single-sign-on/_static/image22.png)

Estamos usando un servidor de inicio de sesión de AD principalmente porque ya hemos configurado ADFS antes de que Azure AD estuviera disponible, pero el proceso de inicio de sesión pasa a través de un directorio Azure AD en la nube. En la nube se colocan los documentos importantes, el control de código fuente, los archivos de administración de rendimiento, los informes de ventas, etc., y se usa esta misma solución para protegerlos.

## <a name="create-an-aspnet-app-that-uses-azure-ad-for-single-sign-on"></a>Creación de una aplicación de ASP.NET que usa Azure AD para el inicio de sesión único

Visual Studio hace que sea muy fácil crear una aplicación que use Azure AD para el inicio de sesión único, como puede ver en algunas capturas de pantalla.

Al crear una nueva aplicación de ASP.NET, ya sea MVC o Web Forms, el método de autenticación predeterminado es ASP.NET Identity. Para cambiarlo a Azure AD, haga clic en un botón **cambiar autenticación** .

![Cambiar autenticación](single-sign-on/_static/image23.png)

Seleccione cuentas de la organización, escriba el nombre de dominio y, a continuación, seleccione Inicio de sesión único.

![Cuadro de diálogo Configurar autenticación](single-sign-on/_static/image24.png)

También puede conceder a la aplicación permiso de lectura o lectura/escritura para los datos de directorio. Si lo hace, puede usar la API de [REST de Azure Graph](https://msdn.microsoft.com/library/windowsazure/hh974476.aspx) para buscar el número de teléfono de los usuarios, averiguar si están en la oficina, Cuándo se iniciaron por última vez, etc.

Eso es todo lo que tiene que hacer: Visual Studio solicita las credenciales de un administrador para el inquilino de Azure AD y, a continuación, configura el proyecto y el inquilino de Azure AD para la nueva aplicación.

Al ejecutar el proyecto, verá una página de inicio de sesión y podrá iniciar sesión con las credenciales de un usuario en el directorio de Azure AD.

![Inicio de sesión de la cuenta de organización](single-sign-on/_static/image25.png)

![Sesión iniciada](single-sign-on/_static/image26.png)

Al implementar la aplicación en Azure, lo único que tiene que hacer es activar la casilla **Habilitar autenticación** de la organización y, una vez más, Visual Studio se encarga de toda la configuración.

![Publicar sitio web](single-sign-on/_static/image27.png)

Estas capturas de pantalla provienen de un tutorial paso a paso completo que muestra cómo compilar una aplicación que usa Azure AD autenticación: [desarrollo de aplicaciones de ASP.net con Azure Active Directory](../../../../identity/overview/getting-started/developing-aspnet-apps-with-windows-azure-active-directory.md).

## <a name="summary"></a>Resumen

En este capítulo vio que Azure Active Directory, Visual Studio y ASP.NET, facilitan la configuración del inicio de sesión único en aplicaciones de Internet para los usuarios de su organización. Los usuarios pueden iniciar sesión en aplicaciones de Internet con las mismas credenciales que usan para iniciar sesión con Active Directory de la red interna.

En el [siguiente capítulo](data-storage-options.md) se examinan las opciones de almacenamiento de datos disponibles para una aplicación en la nube.

<a id="resources"></a>
## <a name="resources"></a>Recursos

Para obtener más información, consulte los siguientes recursos:

- [Documentación de Azure Active Directory](https://docs.microsoft.com/azure/active-directory/). Página del portal de Azure AD documentación en el sitio de windowsazure.com. Para ver tutoriales paso a paso, consulte la sección **desarrollar** .
- [Azure multi-factor Authentication](https://docs.microsoft.com/azure/multi-factor-authentication/). Página del portal para obtener documentación sobre la autenticación multifactor en Azure.
- [Opciones de autenticación de cuenta de organización](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#orgauthoptions). Explicación de las opciones de autenticación de Azure AD en el cuadro de diálogo Visual Studio 2013 New-Project.
- [Patrones y prácticas de Microsoft: patrón de identidad federada](https://msdn.microsoft.com/library/dn589790.aspx).
- [Cómo: instalar la herramienta de sincronización de Azure Active Directory](https://social.technet.microsoft.com/wiki/contents/articles/19098.howto-install-the-windows-azure-active-directory-sync-tool-now-with-pictures.aspx).
- [Mapa de contenido de Servicios de federación de Active Directory (AD FS) 2,0](https://social.technet.microsoft.com/wiki/contents/articles/2735.ad-fs-2-0-content-map.aspx). Vínculos a documentación sobre ADFS 2,0.
- [Autorización basada en roles y basada en ACL en una aplicación de Windows Azure ad](https://code.msdn.microsoft.com/Role-Based-and-ACL-Based-86ad71a1). Aplicación de ejemplo.
- [Azure Active Directory Graph API blog](https://blogs.msdn.com/b/aadgraphteam/).
- [Access Control en BYOD y en la integración de directorios en una infraestructura de identidad híbrida](https://channel9.msdn.com/Events/TechEd/NorthAmerica/2014/PCIT-B213#fbid=). Vídeo de sesión de Tech Ed 2014 de gayana Bagdasaryan.

> [!div class="step-by-step"]
> [Anterior](web-development-best-practices.md)
> [Siguiente](data-storage-options.md)
