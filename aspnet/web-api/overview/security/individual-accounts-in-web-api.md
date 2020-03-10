---
uid: web-api/overview/security/individual-accounts-in-web-api
title: Protección de una API Web con cuentas individuales e inicio de sesión local en ASP.NET Web API 2,2 | Microsoft Docs
author: MikeWasson
description: En este tema se muestra cómo proteger una API Web mediante OAuth2 para autenticarse en una base de datos de pertenencia. Versiones de software usadas en el tutorial de Visual Studio 201...
ms.author: riande
ms.date: 10/15/2014
ms.assetid: 92c84846-f0ea-4b5e-94b6-5004874eb060
msc.legacyurl: /web-api/overview/security/individual-accounts-in-web-api
msc.type: authoredcontent
ms.openlocfilehash: 7492c4aa4c2a0a8aeed64c3462bda8fc51f35a6b
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78447181"
---
# <a name="secure-a-web-api-with-individual-accounts-and-local-login-in-aspnet-web-api-22"></a>Protección de una API Web con cuentas individuales e inicio de sesión local en ASP.NET Web API 2,2

por [Mike Wasson](https://github.com/MikeWasson)

[Descarga de la aplicación de ejemplo](https://github.com/MikeWasson/LocalAccountsApp)

> En este tema se muestra cómo proteger una API Web mediante OAuth2 para autenticarse en una base de datos de pertenencia.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versiones de software usadas en el tutorial
> 
> 
> - [Visual Studio 2013 Update 3](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - [API Web 2,2](../releases/whats-new-in-aspnet-web-api-22.md)
> - [ASP.NET Identity 2,1](../../../identity/index.md)

En Visual Studio 2013, la plantilla de proyecto de Web API ofrece tres opciones para la autenticación:

- **Cuentas individuales.** La aplicación utiliza una base de datos de pertenencia.
- **Cuentas de la organización.** Los usuarios inician sesión con sus credenciales de Azure Active Directory, Office 365 o local Active Directory.
- **Autenticación de Windows.** Esta opción está destinada a las aplicaciones de intranet y utiliza el módulo de IIS de autenticación de Windows.

Para obtener más información sobre estas opciones, vea [crear proyectos Web de ASP.net en Visual Studio 2013](../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#auth).

Las cuentas individuales proporcionan dos maneras para que un usuario inicie sesión:

- **Inicio de sesión local**. El usuario se registra en el sitio, escribiendo un nombre de usuario y una contraseña. La aplicación almacena el hash de contraseña en la base de datos de pertenencia. Cuando el usuario inicia sesión, el sistema ASP.NET Identity comprueba la contraseña.
- **Inicio de sesión social**. El usuario inicia sesión con un servicio externo, como Facebook, Microsoft o Google. La aplicación todavía crea una entrada para el usuario en la base de datos de pertenencia, pero no almacena ninguna credencial. El usuario se autentica al iniciar sesión en el servicio externo.

En este artículo se examina el escenario de inicio de sesión local. Para el inicio de sesión local y social, la API Web usa OAuth2 para autenticar las solicitudes. Sin embargo, los flujos de credenciales son diferentes para el inicio de sesión local y social.

En este artículo, se muestra una aplicación sencilla que permite al usuario iniciar sesión y enviar llamadas AJAX autenticadas a una API Web. Puede descargar el código de ejemplo [aquí](https://github.com/MikeWasson/LocalAccountsApp). El archivo Léame describe cómo crear el ejemplo desde cero en Visual Studio.

[![](individual-accounts-in-web-api/_static/image2.png)](individual-accounts-in-web-api/_static/image1.png)

La aplicación de ejemplo usa Knockout. js para el enlace de datos y jQuery para enviar solicitudes AJAX. Me centraremos en las llamadas de AJAX, por lo que no necesita conocer Knockout. js para este artículo.

A lo largo del proceso, describiré:

- Lo que hace la aplicación en el lado cliente.
- Lo que ocurre en el servidor.
- Tráfico HTTP en el centro.

En primer lugar, es necesario definir cierta terminología de OAuth2.

- *Recurso*. Parte de los datos que se pueden proteger.
- *Servidor de recursos*. Servidor que hospeda el recurso.
- *Propietario del recurso*. Entidad que puede conceder permiso para obtener acceso a un recurso. (Normalmente, el usuario).
- *Cliente*: la aplicación que desea tener acceso al recurso. En este artículo, el cliente es un explorador Web.
- *Token de acceso*. Un token que concede acceso a un recurso.
- *Token de portador*. Un tipo determinado de token de acceso, con la propiedad de que cualquier usuario puede usar el token. En otras palabras, un cliente no necesita una clave criptográfica u otro secreto para usar un token de portador. Por ese motivo, los tokens de portador solo deben usarse a través de HTTPS y deben tener tiempos de expiración relativamente cortos.
- *Servidor de autorización*. Un servidor que proporciona tokens de acceso.

Una aplicación puede actuar como servidor de autorización y servidor de recursos. La plantilla de proyecto de Web API sigue este patrón.

## <a name="local-login-credential-flow"></a>Flujo de credenciales de inicio de sesión local

Para el inicio de sesión local, la API Web usa el [flujo de contraseña del propietario del recurso](http://oauthlib.readthedocs.org/en/latest/oauth2/grants/password.html) definido en OAuth2.

1. El usuario escribe un nombre y una contraseña en el cliente.
2. El cliente envía estas credenciales al servidor de autorización.
3. El servidor de autorización autentica las credenciales y devuelve un token de acceso.
4. Para tener acceso a un recurso protegido, el cliente incluye el token de acceso en el encabezado Authorization de la solicitud HTTP.

![](individual-accounts-in-web-api/_static/image3.png)

Al seleccionar **cuentas individuales** en la plantilla de proyecto de API Web, el proyecto incluye un servidor de autorización que valida las credenciales de usuario y emite tokens. En el diagrama siguiente se muestra el mismo flujo de credenciales en términos de componentes de la API Web.

![](individual-accounts-in-web-api/_static/image4.png)

En este escenario, los controladores de API Web actúan como servidores de recursos. Un filtro de autenticación valida los tokens de acceso y se usa el atributo **[Authorize]** para proteger un recurso. Cuando un controlador o una acción tienen el atributo **[Authorize]** , se deben autenticar todas las solicitudes a ese controlador o acción. De lo contrario, se deniega la autorización y la API Web devuelve un error 401 (no autorizado).

El servidor de autorización y el filtro de autenticación llaman a un componente de [middleware OWIN](../../../aspnet/overview/owin-and-katana/an-overview-of-project-katana.md) que controla los detalles de OAuth2. Describiré el diseño con más detalle más adelante en este tutorial.

## <a name="sending-an-unauthorized-request"></a>Envío de una solicitud no autorizada

Para empezar, ejecute la aplicación y haga clic en el botón **llamar API** . Cuando se complete la solicitud, debería ver un mensaje de error en el cuadro **resultado** . Esto se debe a que la solicitud no contiene un token de acceso, por lo que la solicitud no está autorizada.

[![](individual-accounts-in-web-api/_static/image6.png)](individual-accounts-in-web-api/_static/image5.png)

El botón **Call API** envía una solicitud Ajax a ~/API/Values, que invoca una acción del controlador de API Web. Esta es la sección del código JavaScript que envía la solicitud de AJAX. En la aplicación de ejemplo, todo el código de aplicación de JavaScript se encuentra en el archivo Scripts\app.js.

[!code-javascript[Main](individual-accounts-in-web-api/samples/sample1.js)]

Hasta que el usuario inicia sesión, no hay ningún token de portador y, por lo tanto, no hay ningún encabezado de autorización en la solicitud. Esto hace que la solicitud devuelva un error 401.

Esta es la solicitud HTTP. (Usé [Fiddler](http://www.telerik.com/fiddler) para capturar el tráfico http).

[!code-console[Main](individual-accounts-in-web-api/samples/sample2.cmd)]

Respuesta HTTP:

[!code-console[Main](individual-accounts-in-web-api/samples/sample3.cmd?highlight=1,4)]

Tenga en cuenta que la respuesta incluye un encabezado WWW-Authenticate con el desafío establecido en Bearer. Que indica que el servidor espera un token de portador.

## <a name="register-a-user"></a>Registrar un usuario

En la sección de **registro** de la aplicación, escriba un correo electrónico y una contraseña y haga clic en el botón **registrar** .

No es necesario usar una dirección de correo electrónico válida para este ejemplo, pero una aplicación real confirmaría la dirección. (Consulte [creación de una aplicación Web de ASP.NET MVC 5 segura con inicio de sesión, confirmación de correo electrónico y restablecimiento de contraseña](../../../mvc/overview/security/create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset.md)). Para la contraseña, use algo como "Contraseña1!", con una letra mayúscula, una letra minúscula, un número y un carácter no alfanumérico. Para simplificar la aplicación, he dejado la validación del lado cliente, por lo que si hay un problema con el formato de la contraseña, recibirá un error 400 (solicitud incorrecta).

[![](individual-accounts-in-web-api/_static/image8.png)](individual-accounts-in-web-api/_static/image7.png)

El botón **registrar** envía una solicitud post a ~/API/Account/Register/. El cuerpo de la solicitud es un objeto JSON que contiene el nombre y la contraseña. Este es el código JavaScript que envía la solicitud:

[!code-javascript[Main](individual-accounts-in-web-api/samples/sample4.js)]

Solicitud HTTP:

[!code-console[Main](individual-accounts-in-web-api/samples/sample5.cmd?highlight=5,10)]

Respuesta HTTP:

[!code-console[Main](individual-accounts-in-web-api/samples/sample6.cmd)]

La clase `AccountController` controla esta solicitud. Internamente, `AccountController` usa ASP.NET Identity para administrar la base de datos de pertenencia.

Si ejecuta la aplicación localmente desde Visual Studio, las cuentas de usuario se almacenan en LocalDB, en la tabla AspNetUsers. Para ver las tablas en Visual Studio, haga clic en el menú **Ver** , seleccione **Explorador de servidores**y, a continuación, expanda **conexiones de datos**.

![](individual-accounts-in-web-api/_static/image9.png)

## <a name="get-an-access-token"></a>Obtención de un token de acceso

Hasta ahora no hemos hecho nada, pero ahora veremos el servidor de autorización de OAuth en acción, cuando se solicita un token de acceso. En el área **iniciar sesión** de la aplicación de ejemplo, escriba el correo electrónico y la contraseña y haga clic en **iniciar sesión**.

[![](individual-accounts-in-web-api/_static/image11.png)](individual-accounts-in-web-api/_static/image10.png)

El botón **iniciar sesión** envía una solicitud al punto de conexión del token. El cuerpo de la solicitud contiene los siguientes datos con codificación de dirección URL de formulario:

- conceder\_tipo: "contraseña"
- nombre de usuario: &lt;el correo electrónico del usuario&gt;
- contraseña: &lt;contraseña&gt;

Este es el código JavaScript que envía la solicitud de AJAX:

[!code-javascript[Main](individual-accounts-in-web-api/samples/sample7.js?highlight=14)]

Si la solicitud se realiza correctamente, el servidor de autorización devuelve un token de acceso en el cuerpo de la respuesta. Observe que almacenamos el token en el almacenamiento de sesión, para usarlo más adelante al enviar solicitudes a la API. A diferencia de algunas formas de autenticación (por ejemplo, la autenticación basada en cookies), el explorador no incluirá automáticamente el token de acceso en las solicitudes posteriores. La aplicación debe hacerlo explícitamente. Esto es algo bueno, ya que limita las [vulnerabilidades CSRF](preventing-cross-site-request-forgery-csrf-attacks.md).

Solicitud HTTP:

[!code-console[Main](individual-accounts-in-web-api/samples/sample8.cmd?highlight=5,10)]

Puede ver que la solicitud contiene las credenciales del usuario. *Debe* usar https para proporcionar seguridad de la capa de transporte.

Respuesta HTTP:

[!code-console[Main](individual-accounts-in-web-api/samples/sample9.cmd?highlight=8)]

Para mejorar la legibilidad, he aplicado sangría al JSON y he truncado el token de acceso, que es bastante largo.

Las propiedades `access_token`, `token_type`y `expires_in` se definen mediante la especificación de OAuth2. Las demás propiedades (`userName`, `.issued`y `.expires`) son solo con fines informativos. Puede encontrar el código que agrega las propiedades adicionales en el método `TokenEndpoint`, en el archivo/Providers/ApplicationOAuthProvider.cs.

## <a name="send-an-authenticated-request"></a>Enviar una solicitud autenticada

Ahora que tenemos un token de portador, podemos realizar una solicitud autenticada a la API. Esto se hace estableciendo el encabezado Authorization en la solicitud. Haga clic de nuevo en el botón **llamar a API** para verlo.

[![](individual-accounts-in-web-api/_static/image13.png)](individual-accounts-in-web-api/_static/image12.png)

Solicitud HTTP:

[!code-console[Main](individual-accounts-in-web-api/samples/sample10.cmd?highlight=5)]

Respuesta HTTP:

[!code-console[Main](individual-accounts-in-web-api/samples/sample11.cmd)]

## <a name="log-out"></a>Cerrar sesión

Dado que el explorador no almacena en caché las credenciales o el token de acceso, el registro es simplemente una cuestión de "olvidar" el token, ya que se quita del almacenamiento de sesión:

[!code-javascript[Main](individual-accounts-in-web-api/samples/sample12.js)]

## <a name="understanding-the-individual-accounts-project-template"></a>Descripción de la plantilla de proyecto de cuentas individuales

Al seleccionar **cuentas individuales** en la plantilla de proyecto de aplicación Web ASP.net, el proyecto incluye:

- Un servidor de autorización de OAuth2.
- Un punto de conexión de API Web para administrar cuentas de usuario
- Un modelo EF para almacenar cuentas de usuario.

Estas son las clases de aplicación principales que implementan estas características:

- `AccountController`Operador Proporciona un punto de conexión de API Web para administrar cuentas de usuario. La acción `Register` es la única que usamos en este tutorial. Otros métodos de la clase admiten el restablecimiento de contraseña, inicios de sesión sociales y otras funciones.
- `ApplicationUser`, definido en/Models/IdentityModels.cs. Esta clase es el modelo EF para las cuentas de usuario en la base de datos de pertenencia.
- `ApplicationUserManager`, definido en/App\_Start/IdentityConfig. CS esta clase se deriva de [UserManager](https://msdn.microsoft.com/library/dn613290.aspx) y realiza operaciones en las cuentas de usuario, como la creación de un nuevo usuario, la comprobación de contraseñas, etc., y conserva automáticamente los cambios en la base de datos.
- `ApplicationOAuthProvider`Operador Este objeto se conecta al middleware OWIN y procesa los eventos generados por el middleware. Se deriva de [oauthauthorizationserverprovider consiste](https://msdn.microsoft.com/library/microsoft.owin.security.oauth.oauthauthorizationserverprovider.aspx).

![](individual-accounts-in-web-api/_static/image14.png)

### <a name="configuring-the-authorization-server"></a>Configuración del servidor de autorización

En StartupAuth.cs, el código siguiente configura el servidor de autorización de OAuth2.

[!code-csharp[Main](individual-accounts-in-web-api/samples/sample13.cs)]

La propiedad `TokenEndpointPath` es la ruta de acceso URL al punto de conexión del servidor de autorización. Esta es la dirección URL que usa la aplicación para obtener los tokens de portador.

La propiedad `Provider` especifica un proveedor que se conecta al middleware OWIN y procesa los eventos generados por el middleware.

Este es el flujo básico cuando la aplicación quiere obtener un token:

1. Para obtener un token de acceso, la aplicación envía una solicitud a ~/token.
2. El middleware de OAuth llama a `GrantResourceOwnerCredentials` en el proveedor.
3. El proveedor llama a la `ApplicationUserManager` para validar las credenciales y crear una identidad de notificaciones.
4. Si esto se realiza correctamente, el proveedor crea un vale de autenticación, que se usa para generar el token.

[![](individual-accounts-in-web-api/_static/image16.png)](individual-accounts-in-web-api/_static/image15.png)

El middleware de OAuth no sabe nada sobre las cuentas de usuario. El proveedor se comunica entre el middleware y el ASP.NET Identity. Para obtener más información sobre la implementación del servidor de autorización, consulte [OWIN OAuth 2,0 Authorization Server](../../../aspnet/overview/owin-and-katana/owin-oauth-20-authorization-server.md).

### <a name="configuring-web-api-to-use-bearer-tokens"></a>Configuración de la API Web para usar tokens de portador

En el método `WebApiConfig.Register`, el código siguiente configura la autenticación para la canalización de la API Web:

[!code-csharp[Main](individual-accounts-in-web-api/samples/sample14.cs)]

La clase **HostAuthenticationFilter** permite la autenticación mediante tokens de portador.

El método **SuppressDefaultHostAuthentication** indica a la API Web que omita cualquier autenticación que se produzca antes de que la solicitud llegue a la canalización de API Web, ya sea por IIS o por middleware de OWIN. De este modo, se puede restringir API Web para la autenticación solo con tokens de portador.

> [!NOTE]
> En concreto, la parte de MVC de la aplicación podría usar la autenticación de formularios, que almacena las credenciales en una cookie. La autenticación basada en cookies requiere el uso de tokens antifalsificación para evitar ataques CSRF. Este es un problema para las API Web, ya que no hay ninguna manera cómoda de que la API Web envíe el token antifalsificación al cliente. (Para obtener más información sobre este problema, vea [evitar ataques CSRF en la API Web](preventing-cross-site-request-forgery-csrf-attacks.md)). La llamada a **SuppressDefaultHostAuthentication** garantiza que la API Web no sea vulnerable a ataques CSRF de credenciales almacenadas en cookies.

Cuando el cliente solicita un recurso protegido, esto es lo que sucede en la canalización de la API Web:

1. El filtro **HostAuthentication** llama al middleware de OAuth para validar el token.
2. El middleware convierte el token en una identidad de notificaciones.
3. En este momento, la solicitud se *autentica* pero no se *autoriza*.
4. El filtro de autorización examina la identidad de notificaciones. Si las notificaciones autorizan al usuario para ese recurso, la solicitud está autorizada. De forma predeterminada, el atributo **[Authorize]** autorizará cualquier solicitud que se autentique. Sin embargo, puede autorizar por rol o por otras notificaciones. Para obtener más información, consulte [autenticación y autorización en Web API](authentication-and-authorization-in-aspnet-web-api.md).
5. Si los pasos anteriores se realizan correctamente, el controlador devuelve el recurso protegido. De lo contrario, el cliente recibe un error 401 (no autorizado).

[![](individual-accounts-in-web-api/_static/image18.png)](individual-accounts-in-web-api/_static/image17.png)

## <a name="additional-resources"></a>Recursos adicionales

- [ASP.NET Identity](../../../identity/index.md)
- [Descripción de las características de seguridad en la plantilla de Spa para VS2013 RC](https://blogs.msdn.com/b/webdev/archive/2013/09/20/understanding-security-features-in-spa-template.aspx). Entrada de blog de MSDN de Hongye Sun.
- [Dessección de la plantilla de cuentas individuales de la API Web, parte 2: cuentas locales](http://leastprivilege.com/2013/11/26/dissecting-the-web-api-individual-accounts-templatepart-2-local-accounts/). Entrada de blog de Dominick Baier.
- [Autenticación de host y API Web con OWIN](http://brockallen.com/2013/10/27/host-authentication-and-web-api-with-owin-and-active-vs-passive-authentication-middleware/). Una buena explicación de `SuppressDefaultHostAuthentication` y `HostAuthenticationFilter` de Brock Allen.
- [Personalización de la información del perfil en ASP.net Identity en las plantillas de VS 2013](https://blogs.msdn.com/b/webdev/archive/2013/10/16/customizing-profile-information-in-asp-net-identity-in-vs-2013-templates.aspx). Publicación de blog de MSDN de Pranav Rastogi.
- [Por administración de la duración de las solicitudes para la clase UserManager en ASP.net Identity](https://blogs.msdn.com/b/webdev/archive/2014/02/12/per-request-lifetime-management-for-usermanager-class-in-asp-net-identity.aspx). Publicación de blog de MSDN de Suhas Joshi, con una buena explicación de la clase `UserManager`.
