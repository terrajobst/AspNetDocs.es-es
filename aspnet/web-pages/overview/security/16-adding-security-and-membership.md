---
uid: web-pages/overview/security/16-adding-security-and-membership
title: Agregar seguridad y pertenencia a un sitio de ASP.NET Web Pages (Razor) | Microsoft Docs
author: Rick-Anderson
description: En este capítulo se muestra cómo proteger el sitio web para que algunas de las páginas estén disponibles solo para los usuarios que inicien sesión. (También verá cómo crear páginas Tha...
ms.author: riande
ms.date: 02/24/2014
ms.assetid: 7a77c2c0-deea-4290-a9c3-97958891758e
msc.legacyurl: /web-pages/overview/security/16-adding-security-and-membership
msc.type: authoredcontent
ms.openlocfilehash: 0be3e767a42939a3c343f6d4a730eb1d9a6b367c
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78509905"
---
# <a name="adding-security-and-membership-to-an-aspnet-web-pages-razor-site"></a>Agregar seguridad y pertenencia a un sitio de ASP.NET Web Pages (Razor)

por [Tom FitzMacken](https://github.com/tfitzmac)

> En este artículo se explica cómo proteger un sitio web de ASP.NET Web Pages (Razor) para que algunas de las páginas estén disponibles solo para los usuarios que inicien sesión. (También verá cómo crear páginas a las que cualquiera pueda tener acceso).
> 
> **Lo que aprenderá:** 
> 
> - Cómo crear un sitio web que tenga una página de registro y una página de inicio de sesión para que en algunas páginas pueda limitar el acceso solo a los miembros.
> - Cómo crear páginas públicas y solo de miembro.
> - Cómo definir roles, que son grupos que tienen permisos de seguridad diferentes en el sitio, y cómo asignar usuarios a un rol.
> - Cómo usar CAPTCHA para evitar que programas automatizados (bots) creen cuentas de miembro.
>   
> 
> Estas son las características de ASP.NET presentadas en el artículo:
> 
> - La plantilla del **sitio de inicio** de WebMatrix.
> - Aplicación auxiliar de `WebSecurity` y `Roles` clase.
> - Aplicación auxiliar de `ReCaptcha`.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versiones de software usadas en el tutorial
> 
> 
> - ASP.NET Web Pages (Razor) 2
> - WebMatrix 3
> - Biblioteca de aplicaciones auxiliares Web de ASP.NET

Puede configurar su sitio web para que los usuarios puedan iniciar sesión en &#8212; él, de modo que el sitio admita la *pertenencia*. Esto puede ser útil por muchas razones. Por ejemplo, es posible que el sitio tenga páginas que solo deben estar disponibles para los miembros. En algunos casos, es posible que sea necesario que los usuarios inicien sesión para poder enviar sus comentarios o dejar un comentario.

Incluso si su sitio web admite la pertenencia, no es necesario que los usuarios inicien sesión antes de usar algunas de las páginas del sitio. Los usuarios que no han iniciado sesión se conocen como *usuarios anónimos*.

Un usuario puede registrarse en el sitio web y, a continuación, iniciar sesión en el sitio. El sitio web requiere un nombre de usuario (una dirección de correo electrónico) y una contraseña para confirmar que los usuarios son quienes dicen ser. Este proceso de inicio de sesión y confirmación de la identidad de un usuario se conoce como *autenticación*.

Puede configurar la seguridad y la pertenencia de maneras diferentes:

- Si usa WebMatrix, una manera fácil es crear como nuevo sitio basado en la plantilla del **sitio de inicio** . Esta plantilla ya está configurada para la seguridad y la pertenencia, y ya tiene una página de registro, una página de inicio de sesión, etc.

    El sitio creado por la plantilla también tiene una opción que permite a los usuarios iniciar sesión mediante un sitio externo como Facebook, Google o Twitter.
- Si desea agregar seguridad a un sitio existente, o si no desea usar la plantilla del sitio de **Inicio** , puede crear su propia página de registro, la página de inicio de sesión, etc.

Este artículo se centra en la primera opción &mdash; cómo agregar seguridad mediante la plantilla **sitio de inicio** . También se proporciona información básica sobre cómo implementar su propia seguridad y, a continuación, se proporcionan vínculos para obtener más información sobre cómo hacerlo. También hay información sobre cómo habilitar los inicios de sesión externos, que se describe con más detalle en un artículo independiente.

## <a name="creating-website-security-using-the-starter-site-template"></a>Crear seguridad de sitio web mediante la plantilla de sitio de inicio

En WebMatrix, puede usar la plantilla **sitio de inicio** para crear un sitio web que contenga lo siguiente:

- Base de datos que se usa para almacenar nombres de usuario y contraseñas para los miembros.
- Página de registro en la que se pueden registrar usuarios anónimos (nuevos).
- Una página de inicio y cierre de sesión.
- Una página de recuperación y restablecimiento de contraseñas.

En el procedimiento siguiente se describe cómo crear el sitio y configurarlo.

1. Inicie WebMatrix y, en la página **Inicio rápido** , seleccione **sitio de plantilla**.
2. Seleccione la plantilla **sitio de inicio** y haga clic en **Aceptar**. WebMatrix crea un nuevo sitio.
3. En el panel izquierdo, haga clic en el selector de área de trabajo **archivos** .
4. En la carpeta raíz de su sitio web, abra el archivo *\_AppStart. cshtml* , que es un archivo especial que se usa para contener la configuración global. Contiene algunas instrucciones que se marcan como comentarios con los caracteres `//`:

    [!code-csharp[Main](16-adding-security-and-membership/samples/sample1.cs)]

    Estas instrucciones configuran la aplicación auxiliar de `WebMail`, que se puede usar para enviar correo electrónico. El sistema de pertenencia puede usar el correo electrónico para enviar mensajes de confirmación cuando los usuarios se registran o cuando desean cambiar sus contraseñas. (Por ejemplo, después de que los usuarios se registren, reciben un correo electrónico que incluye un vínculo en el que pueden hacer clic para finalizar el proceso de registro).

    El envío de correo electrónico requiere acceso a un servidor SMTP, tal como se describe en [Agregar correo electrónico a un sitio ASP.NET Web pages](https://go.microsoft.com/fwlink/?LinkId=202899). Almacenará la configuración de correo electrónico en este archivo central *\_AppStart. cshtml* para que no tenga que codificarla repetidamente en cada página que pueda enviar correo electrónico. (No es necesario configurar los valores de SMTP para configurar una base de datos de registro; solo se necesita la configuración de SMTP si desea validar a los usuarios de su alias de correo electrónico y permitir que los usuarios restablezcan la contraseña olvidada).
5. Quite los comentarios de las instrucciones quitando `//` de delante de cada una de ellas.

    Si no desea configurar la confirmación por correo electrónico, puede omitir este paso y el paso siguiente. Si no se establecen los valores SMTP, la nueva cuenta estará inmediatamente disponible sin un correo electrónico de confirmación.
6. Modifique la siguiente configuración relacionada con el correo electrónico en el código:

   - Establezca `WebMail.SmtpServer` en el nombre del servidor SMTP al que tenga acceso.
   - Deje `WebMail.EnableSsl` establecido en `true`. Esta configuración protege las credenciales que se envían al servidor SMTP mediante su cifrado.
   - Establezca `WebMail.UserName` en el nombre de usuario de la cuenta del servidor SMTP.
   - Establezca `WebMail.Password` en la contraseña de la cuenta del servidor SMTP.
   - Establezca `WebMail.From` en su propia dirección de correo electrónico. Esta es la dirección de correo electrónico desde la que se envía el mensaje.

     > [!NOTE] 
     > 
     > **Sugerencia** Para obtener más información sobre los valores de estas propiedades, consulte Configuración del [correo electrónico](https://go.microsoft.com/fwlink/?LinkID=202906#configuring_email_settings) en [personalizar el comportamiento de todo el sitio para ASP.NET Web pages](https://go.microsoft.com/fwlink/?LinkID=202906).
7. Guarde y cierre *\_AppStart. cshtml*.
8. Ejecute la página *default. cshtml* en un explorador.

    ![seguridad-pertenencia-2](16-adding-security-and-membership/_static/image1.png)

   > [!NOTE]
   > Si ve un error que indica que una propiedad debe ser una instancia de `ExtendedMembershipProvider`, es posible que el sitio no esté configurado para usar el sistema de pertenencia a ASP.NET Web Pages (SimpleMembership). A veces, esto puede ocurrir si el servidor de un proveedor de hospedaje se configura de forma diferente que el servidor local. Para corregir esto, agregue el siguiente elemento al archivo *Web. config* del sitio:
   > 
   > [!code-xml[Main](16-adding-security-and-membership/samples/sample2.xml)]
   > 
   > Agregue este elemento como elemento secundario del elemento `<configuration>` y como elemento del mismo nivel del elemento `<system.web>`.
9. En la esquina superior derecha de la página, haga clic en el vínculo **registrar** . Se muestra la página *Register. cshtml* .
10. Escriba un nombre de usuario y una contraseña y, a continuación, haga clic en **registrar**.

    ![seguridad-pertenencia-3](16-adding-security-and-membership/_static/image2.png)

    Al crear el sitio web a partir de la plantilla del **sitio de inicio** , se creó una base de *datos* denominada *StarterSite. sdf* en la carpeta de datos de la aplicación del sitio\_. Durante el registro, se agrega la información de usuario a la base de datos. Si establece los valores de SMTP, se envía un mensaje a la dirección de correo electrónico que ha usado para que pueda finalizar el registro.

    ![seguridad-pertenencia-4](16-adding-security-and-membership/_static/image3.png)
11. Vaya a su programa de correo electrónico y busque el mensaje, que tendrá el código de confirmación y un hipervínculo al sitio.
12. Haga clic en el hipervínculo para activar su cuenta. El hipervínculo de confirmación abre una página de confirmación de registro.

    ![seguridad-pertenencia-5](16-adding-security-and-membership/_static/image4.png)
13. Haga clic en el vínculo **Inicio de sesión** y, a continuación, inicie sesión con la cuenta que registró.

      Después de iniciar sesión, los vínculos de **Inicio de sesión** y **registro** se sustituyen por un vínculo de **cierre** de sesión. El nombre de inicio de sesión se muestra como un vínculo. (El vínculo le permite ir a una página donde puede cambiar la contraseña).

      ![seguridad-pertenencia-6](16-adding-security-and-membership/_static/image5.png)

      > [!NOTE]
      > De forma predeterminada, las páginas Web ASP.NET envían credenciales al servidor en texto no cifrado (como texto legible). Un sitio de producción debe usar HTTP seguro (https://, también conocido como *capa de sockets seguros* o SSL) para cifrar la información confidencial intercambiada con el servidor. Puede solicitar que los mensajes de correo electrónico se envíen mediante SSL estableciendo `WebMail.EnableSsl=true` como en el ejemplo anterior. Para obtener más información acerca de SSL, consulte protección de las [comunicaciones web: certificados, SSL y https://](https://go.microsoft.com/fwlink/?LinkId=208660).

## <a name="additional-membership-functionality-in-the-site"></a>Funcionalidad de pertenencia adicional en el sitio

El sitio contiene otras funciones que permiten a los usuarios administrar sus cuentas. Los usuarios pueden hacer lo siguiente:

- Cambiar sus contraseñas. Después de iniciar sesión, pueden hacer clic en el nombre de usuario (que es un vínculo). Esto lo lleva a una página en la que puede crear una nueva contraseña (*account/ChangePassword. cshtml*).
- Recuperar una contraseña olvidada. En la página de inicio de sesión, hay un vínculo ( **¿olvidó su contraseña?** ) que lleva a los usuarios a una página (*account/ForgotPassword. cshtml*) donde pueden escribir una dirección de correo electrónico. El sitio les envía un mensaje de correo electrónico con un vínculo en el que se puede hacer clic para establecer una nueva contraseña (*account/PasswordReset. cshtml*).

También puede permitir que los usuarios puedan iniciar sesión con un sitio externo, como se explica más adelante.

<a id="Creating_a_Members-Only_Page"></a>
## <a name="creating-a-members-only-page"></a>Crear una página de solo miembros

En ese momento, cualquier usuario puede ir a cualquier página de su sitio Web. Pero es posible que desee tener páginas que solo estén disponibles para las personas que han iniciado sesión (es decir, a los miembros). ASP.NET permite crear páginas a las que solo pueden tener acceso los miembros que han iniciado sesión. Normalmente, si los usuarios anónimos intentan obtener acceso a una página de solo miembros, se redirigen a la página de inicio de sesión.

En este procedimiento, creará una carpeta que contendrá páginas que solo están disponibles para los usuarios que han iniciado sesión.

1. En la raíz del sitio, cree una nueva carpeta. (En la cinta de opciones, haga clic en la flecha situada debajo de **nuevo** y elija **nueva carpeta**).
2. Asigne a la nueva carpeta el nombre *miembros*.
3. Dentro de la carpeta *Members* , cree una nueva página y asígnele el nombre *MembersInformation. cshtml*.
4. Reemplace el contenido existente por el siguiente código y marcado:

    [!code-cshtml[Main](16-adding-security-and-membership/samples/sample3.cshtml)]

    Este código prueba la propiedad `IsAuthenticated` del objeto `WebSecurity`, que devuelve `true` si el usuario ha iniciado sesión. Si el usuario no ha iniciado sesión, el código llama a `Response.Redirect` para enviar al usuario a la página *login. cshtml* de la carpeta de la *cuenta* .

    La dirección URL de la redirección incluye un valor de cadena de consulta `returnUrl` que usa `Request.Url.LocalPath` para establecer la ruta de acceso de la página actual. Si establece el valor de `returnUrl` en la cadena de consulta de este modo (y si la dirección URL de retorno es una ruta de acceso local), la página de inicio de sesión devolverá los usuarios a esta página después de iniciar sesión.

    El código también establece *\_página SiteLayout. cshtml* como su página de diseño. (Para obtener más información sobre las páginas de diseño, vea [crear un diseño coherente en sitios de ASP.NET Web pages](https://go.microsoft.com/fwlink/?LinkId=202891)).
5. Ejecute el sitio. Si aún tiene iniciada la sesión, haga clic en el botón **Cerrar sesión** en la parte superior de la página.
6. En el explorador, solicite la página */Members/MembersInformation*. Por ejemplo, la dirección URL podría tener el siguiente aspecto:

    `http://localhost:38366/Members/MembersInformation`

    (El número de puerto (38366) probablemente será diferente en la dirección URL).

    Se le redirigirá a la página *login. cshtml* , ya que no ha iniciado sesión.
7. Inicie sesión con la cuenta que creó anteriormente. Se le redirigirá a la página *MembersInformation* . Como ha iniciado sesión, esta vez verá el contenido de la página.

Para proteger el acceso a varias páginas, puede hacer lo siguiente:

- Agregue la comprobación de seguridad a cada página.
- Cree una *\_página PageStart. cshtml* en la carpeta donde mantiene las páginas protegidas y agregue la comprobación de seguridad allí. La página *\_PageStart. cshtml* actúa como un tipo de página global para todas las páginas de la carpeta. Esta técnica se explica con más detalle en [personalizar el comportamiento de todo el sitio para ASP.NET Web pages](https://go.microsoft.com/fwlink/?LinkId=202906#Using__PageStart.cshtml_to_Restrict_Folder_Access).

## <a name="creating-security-for-groups-of-users-roles"></a>Crear seguridad para grupos de usuarios (roles)

Si el sitio tiene muchos miembros, no es eficaz comprobar los permisos de cada usuario individualmente antes de permitirles ver una página. Lo que puede hacer es crear grupos o *roles*a los que pertenecen los miembros individuales. A continuación, puede comprobar los permisos según el rol. En esta sección, creará un rol de&quot; de administración de &quot;y, a continuación, creará una página a la que puedan tener acceso los usuarios que pertenezcan a ese rol.

El sistema de pertenencia de ASP.NET está configurado para admitir roles. Sin embargo, a diferencia del registro de pertenencia e inicio de sesión, la plantilla del **sitio de inicio** no contiene páginas que le ayuden a administrar los roles. (La administración de roles es una tarea administrativa en lugar de una tarea de usuario). Sin embargo, puede agregar grupos directamente en la base de datos de pertenencia en WebMatrix.

1. En WebMatrix, haga clic en el selector de área de trabajo **bases de datos** .
2. En el panel izquierdo, abra el nodo *StarterSite. sdf* , abra el nodo **tablas** y, a continuación, haga doble clic en la tabla *páginas web\_roles* .

    ![seguridad-pertenencia-7](16-adding-security-and-membership/_static/image6.png)
3. Agregue un rol denominado &quot;admin&quot;. El campo *RoleId* se rellena automáticamente. (Es la clave principal y se ha establecido como un campo de identificación, como se explica en [Introducción al trabajo con una base de datos en ASP.NET Web pages sitios](https://go.microsoft.com/fwlink/?LinkId=202893)).
4. Tome nota de cuál es el valor del campo *RoleId* . (Si se trata del primer rol que está definiendo, será 1).

    ![seguridad-pertenencia-8](16-adding-security-and-membership/_static/image7.png)
5. Cierre la tabla *páginas web\_roles* .
6. Abra la tabla *userprofile* .
7. Anote el valor de *userid* de uno o varios de los usuarios de la tabla y, a continuación, cierre la tabla.
8. Abra la tabla *webpages\_UserInRoles* y escriba un *userid* y un valor de *RoleID* en la tabla. Por ejemplo, para poner el usuario 2 en el rol de&quot; de administración de &quot;, debe escribir estos valores:

    ![seguridad-pertenencia-9](16-adding-security-and-membership/_static/image8.png)
9. Cierre la tabla *webpages\_UsersInRoles* .

    Ahora que ha definido los roles, puede configurar una página que sea accesible para los usuarios que están en ese rol.
10. En la carpeta raíz del sitio web, cree una nueva página denominada *AdminError. cshtml* y reemplace el contenido existente por el código siguiente. Esta será la página a la que se redirigirán los usuarios si no se les permite el acceso a una página.

    [!code-cshtml[Main](16-adding-security-and-membership/samples/sample4.cshtml)]
11. En la carpeta raíz del sitio web, cree una nueva página denominada *AdminOnly. cshtml* y reemplace el código existente por el código siguiente:

    [!code-cshtml[Main](16-adding-security-and-membership/samples/sample5.cshtml)]

    El método `Roles.IsUserInRole` devuelve `true` si el usuario actual es miembro del rol especificado (en este caso, el rol "admin").
12. Ejecute *default. cshtml* en un explorador, pero no inicie sesión. (Si ya ha iniciado sesión, cierre la sesión).
13. En la barra de direcciones del explorador, agregue *AdminOnly* en la dirección URL. (Es decir, solicite el archivo *AdminOnly. cshtml* ). Se le redirigirá a la página *AdminError. cshtml* porque no ha iniciado sesión actualmente como usuario en el rol de&quot; de administración de &quot;.
14. Vuelva a *default. cshtml* e inicie sesión como el usuario que agregó al rol de&quot; de administración de &quot;.
15. Vaya a la página *AdminOnly. cshtml* . Esta vez, verá la página.

## <a name="preventing-automated-programs-from-joining-your-website"></a>Impedir que los programas automatizados se unan a su sitio web

La página de inicio de sesión no detendrá los programas automatizados (a veces denominados robots o *bots* *Web* ) del registro en el sitio Web. En este procedimiento se describe cómo habilitar una prueba de ReCaptcha para la página de registro.

![/media/38777/ch16securitymembership-18.jpg](16-adding-security-and-membership/_static/image1.jpg)

1. Registre el sitio web en ReCaptcha.Net ([http://recaptcha.net](http://recaptcha.net)). Cuando haya completado el registro, obtendrá una clave pública y una clave privada.
2. Agregue la biblioteca de aplicaciones auxiliares Web de ASP.NET a su sitio web, tal como se describe en [instalación de aplicaciones auxiliares en un sitio de ASP.NET Web pages](https://go.microsoft.com/fwlink/?LinkId=252372), si no lo ha hecho ya.
3. En la carpeta de la *cuenta* , abra el archivo denominado *Register. cshtml*.
4. En el código que se encuentra en la parte superior de la página, busque las siguientes líneas y quite las marcas de comentario mediante la eliminación de los caracteres de comentario `//`:

    [!code-csharp[Main](16-adding-security-and-membership/samples/sample6.cs)]
5. Reemplace `PRIVATE_KEY` por su propia clave privada de ReCaptcha.
6. En el marcado de la página, quite los caracteres de comentario `@*` y `*@` de alrededor de las líneas siguientes en el marcado de página:

    [!code-cshtml[Main](16-adding-security-and-membership/samples/sample7.cshtml)]
7. Reemplace `PUBLIC_KEY` por su clave.
8. Si todavía no lo ha quitado, quite el elemento `<div>` que contiene texto que empieza por "para habilitar la comprobación de CAPTCHA...". (Quite todo el elemento de la `<div>` y su contenido).

9. Ejecute *default. cshtml* en un explorador. Si ha iniciado sesión en el sitio, haga clic en el vínculo **Cerrar sesión** .
10. Haga clic en el vínculo **registrar** y pruebe el registro con la prueba CAPTCHA.

     ![seguridad-pertenencia-10](16-adding-security-and-membership/_static/image9.png)

Para obtener más información acerca de la aplicación auxiliar de `ReCaptcha`, consulte [uso de catpcha para impedir que programas automatizados (bots) usen el sitio web de ASP.net](https://go.microsoft.com/fwlink/?LinkId=251967).

<a id="Additional_Resources"></a>
## <a name="letting-users-log-in-using-an-external-site"></a>Permitir a los usuarios iniciar sesión con un sitio externo

La plantilla del **sitio de inicio** incluye código y marcado que permite a los usuarios iniciar sesión con Facebook, Windows Live, Twitter, Google o Yahoo. De forma predeterminada, esta funcionalidad no está habilitada. El procedimiento general para usar permitir que los usuarios inicien sesión con estos proveedores externos es el siguiente:

- Decida qué sitios externos desea admitir.
- Si es necesario, vaya a ese sitio y configure una aplicación de inicio de sesión. (Por ejemplo, tiene que hacer esto para permitir los inicios de sesión de Facebook).
- En el sitio, configure el proveedor. En la mayoría de los casos, solo tiene que quitar la marca de comentario de código en el archivo *\_AppStart. cshtml* .
- Agregue marcado a la página de registro que permita que las personas se vinculen al sitio externo para iniciar sesión. Normalmente, puede copiar el marcado que necesita y cambiar el texto ligeramente.

Puede encontrar instrucciones paso a paso en el tema [Habilitar el inicio de sesión desde sitios externos en un sitio de ASP.NET Web pages](https://go.microsoft.com/fwlink/?LinkId=251969).

Después de que un usuario inicie sesión desde otro sitio, el usuario vuelve al sitio y *asocia* ese inicio de sesión con el sitio. De hecho, esto crea una entrada de pertenencia en el sitio para el inicio de sesión externo del usuario. Esto le permite usar los medios normales de pertenencia (como roles) con el inicio de sesión externo.

## <a name="adding-security-to-an-existing-website"></a>Agregar seguridad a un sitio web existente

El procedimiento anterior de este artículo se basa en el uso de la plantilla del **sitio de inicio** como base para la seguridad del sitio Web. Si no es práctico empezar a partir de la plantilla del **sitio de inicio** o copiar las páginas correspondientes de un sitio basado en esa plantilla, puede implementar el mismo tipo de seguridad en su propio sitio mediante su codificación. Cree los mismos tipos de páginas (registro, Inicio de sesión, etc.) y, a continuación, use aplicaciones auxiliares y clases para configurar la pertenencia.

El proceso básico se describe en la entrada de blog [la manera más básica de implementar la seguridad de Razor para ASP.net](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2240). La mayor parte del trabajo se realiza mediante los siguientes métodos y propiedades de la aplicación auxiliar de `WebSecurity`:

- [WebSecurty. UserExists](https://msdn.microsoft.com/library/webmatrix.webdata.websecurity.userexists(v=vs.99).aspx), [WebSecurity. CreateUserAndAccount](https://msdn.microsoft.com/library/webmatrix.webdata.websecurity.createuserandaccount(v=vs.99).aspx). Estos métodos permiten determinar si alguien ya está registrado y registrarlos.
- [WebSecurty. IsAuthenticated](https://msdn.microsoft.com/library/webmatrix.webdata.websecurity.isauthenticated(v=vs.99).aspx). Esta propiedad le permite determinar si el usuario actual ha iniciado sesión. Esto resulta útil para redirigir a los usuarios a una página de inicio de sesión si todavía no han iniciado sesión.
- [WebSecurity. login](https://msdn.microsoft.com/library/webmatrix.webdata.websecurity.login(v=vs.99).aspx), [WebSecurity. logout](https://msdn.microsoft.com/library/webmatrix.webdata.websecurity.logout(v=vs.99).aspx). Estos métodos inician una sesión de usuario o de salida.
- [WebSecurity. CurrentUserName](https://msdn.microsoft.com/library/webmatrix.webdata.websecurity.currentusername(v=vs.99).aspx). Esta propiedad es útil para mostrar el nombre de usuario actual (si el usuario ha iniciado sesión).
- [WebSecurity. ConfirmAccount](https://msdn.microsoft.com/library/gg569286(v=vs.99).aspx). Este método es útil si configura la confirmación por correo electrónico para el registro. (Los detalles se describen en la entrada de blog [mediante la característica de confirmación de ASP.NET Web pages seguridad](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2267)).

Para administrar roles, puede usar los [roles](https://msdn.microsoft.com/library/gg538398(v=vs.99).aspx) y las clases de [pertenencia](https://msdn.microsoft.com/library/gg569035(v=vs.99).aspx) , como se describe en la entrada de blog.

## <a name="additional-resources"></a>Recursos adicionales

- [Personalizar el comportamiento de todo el sitio](https://go.microsoft.com/fwlink/?LinkId=202906)
- [Protección de las comunicaciones web: certificados, SSL y https://](https://go.microsoft.com/fwlink/?LinkId=208660)
- [La manera más básica de implementar la seguridad de Razor para ASP.net](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2240) y [usar la característica de confirmación para ASP.NET Web pages seguridad](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2267). Se trata de entradas de blog que describen cómo implementar las características de pertenencia a ASP.NET sin usar la plantilla del **sitio de inicio** .
- [Habilitar el inicio de sesión desde sitios externos en un sitio de ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=251969)
- [Referencia de la API de clase WebSecurity](https://msdn.microsoft.com/library/webmatrix.webdata.websecurity(v=vs.99)) (MSDN)
- [Referencia de la API de clase SimpleRoleProvider](https://msdn.microsoft.com/library/webmatrix.webdata.simpleroleprovider(v=vs.99)) (MSDN)
- [Referencia de la API de clase SimpleMembershipProvider](https://msdn.microsoft.com/library/webmatrix.webdata.simplemembershipprovider(v=vs.99)) (MSDN)
