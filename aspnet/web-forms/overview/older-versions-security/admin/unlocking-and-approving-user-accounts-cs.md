---
uid: web-forms/overview/older-versions-security/admin/unlocking-and-approving-user-accounts-cs
title: Desbloqueo y aprobación de cuentas de usuarioC#() | Microsoft Docs
author: rick-anderson
description: En este tutorial se muestra cómo crear una página web para que los administradores administren los Estados de los usuarios bloqueados y aprobados. También veremos cómo aprobar nuevos usuarios o...
ms.author: riande
ms.date: 04/01/2008
ms.assetid: 5346aab1-9974-489f-a065-ae3883b8a350
msc.legacyurl: /web-forms/overview/older-versions-security/admin/unlocking-and-approving-user-accounts-cs
msc.type: authoredcontent
ms.openlocfilehash: c19f7dfac0ddd12c2b4f3388a71a8ca0f71cbb18
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/28/2019
ms.locfileid: "74580131"
---
# <a name="unlocking-and-approving-user-accounts-c"></a>Desbloquear y autorizar cuentas de usuario (C#)

por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descargar código](https://download.microsoft.com/download/6/0/e/60e1bd94-e5f9-4d5a-a079-f23c98f4f67d/CS.14.zip) o [Descargar PDF](https://download.microsoft.com/download/6/0/e/60e1bd94-e5f9-4d5a-a079-f23c98f4f67d/aspnet_tutorial14_UnlockAndApprove_cs.pdf)

> En este tutorial se muestra cómo crear una página web para que los administradores administren los Estados de los usuarios bloqueados y aprobados. También veremos cómo aprobar nuevos usuarios solo después de haber comprobado su dirección de correo electrónico.

## <a name="introduction"></a>Introducción

Junto con un nombre de usuario, una contraseña y un correo electrónico, cada cuenta de usuario tiene dos campos de estado que determinan si el usuario puede iniciar sesión en el sitio: bloqueado y aprobado. Un usuario se bloquea automáticamente si proporciona credenciales no válidas un número especificado de veces dentro de un número especificado de minutos (la configuración predeterminada bloquea a un usuario después de 5 intentos de inicio de sesión no válidos en 10 minutos). El estado aprobado es útil en escenarios en los que se debe llevar a cabo alguna acción antes de que un nuevo usuario pueda iniciar sesión en el sitio. Por ejemplo, un usuario podría tener que comprobar primero su dirección de correo electrónico o ser aprobada por un administrador antes de poder iniciar sesión.

Dado que un usuario bloqueado o no aprobado no puede iniciar sesión, solo es natural preguntar cómo se pueden restablecer estos Estados. ASP.NET no incluye ninguna funcionalidad integrada ni controles Web para la administración de los Estados de los usuarios bloqueados y aprobados, en parte porque es necesario controlar estas decisiones de sitio por sitio. Algunos sitios pueden aprobar automáticamente todas las cuentas de usuario nuevas (comportamiento predeterminado). Otros usuarios tienen un administrador que aprueba nuevas cuentas o no aprueban a los usuarios hasta que visitan un vínculo que se envía a la dirección de correo electrónico que se le proporcionó al suscribirse. Del mismo modo, algunos sitios pueden bloquear a los usuarios hasta que un administrador restablece su estado, mientras que otros sitios envían un correo electrónico al usuario bloqueado con una dirección URL que pueden visitar para desbloquear su cuenta.

En este tutorial se muestra cómo crear una página web para que los administradores administren los Estados de los usuarios bloqueados y aprobados. También veremos cómo aprobar nuevos usuarios solo después de haber comprobado su dirección de correo electrónico.

## <a name="step-1-managing-users-locked-out-and-approved-statuses"></a>Paso 1: administración de los Estados de los usuarios bloqueados y aprobados

En el <a id="Tutorial12"> </a>tutorial [*creación de una interfaz para seleccionar una cuenta de usuario de muchos*](building-an-interface-to-select-one-user-account-from-many-cs.md) , hemos creado una página con una lista de cada una de las cuentas de usuario en un control GridView paginado y filtrado. En la cuadrícula se muestra el nombre y el correo electrónico de cada usuario, sus Estados aprobados y bloqueados, si están en línea y cualquier comentario sobre el usuario. Para administrar los Estados aprobados y bloqueados de los usuarios, podríamos hacer que esta cuadrícula sea editable. Para cambiar el estado aprobado de un usuario, el administrador ubicaría primero la cuenta de usuario y, a continuación, edita la fila de GridView correspondiente, activando o desactivando la casilla aprobado. Como alternativa, podríamos administrar los Estados aprobados y bloqueados a través de una página de ASP.NET independiente.

En este tutorial, vamos a usar dos páginas de ASP.NET: `ManageUsers.aspx` y `UserInformation.aspx`. La idea es que `ManageUsers.aspx` muestra las cuentas de usuario en el sistema, mientras `UserInformation.aspx` permite que el administrador administre los Estados aprobados y bloqueados para un usuario específico. Nuestro primer orden de negocio es aumentar el GridView en `ManageUsers.aspx` para incluir un HyperLinkField, que se representa como una columna de vínculos. Queremos que cada vínculo apunte a `UserInformation.aspx?user=UserName`, donde *username* es el nombre del usuario que se va a editar.

> [!NOTE]
> Si descargó el código para el <a id="Tutorial13"> </a>tutorial [*recuperación y cambio de contraseñas*](recovering-and-changing-passwords-cs.md) , es posible que haya observado que la página `ManageUsers.aspx` ya contiene un conjunto de vínculos "administrar" y la página `UserInformation.aspx` proporciona una interfaz para cambiar la contraseña del usuario seleccionado. Decidí no replicar esa funcionalidad en el código asociado a este tutorial porque funcionaba mediante la elusión de la API de pertenencia y el funcionamiento directo con la SQL Server base de datos para cambiar la contraseña de un usuario. Este tutorial comienza desde cero con la página `UserInformation.aspx`.

### <a name="adding-manage-links-to-theuseraccountsgridview"></a>Agregar vínculos de "administración" al`UserAccounts`GridView

Abra la página `ManageUsers.aspx` y agregue un HyperLinkField al `UserAccounts` GridView. Establezca la propiedad `Text` de HyperLinkField en "administrar" y sus propiedades `DataNavigateUrlFields` y `DataNavigateUrlFormatString` en `UserName` y "UserInformation. aspx? User ={0}", respectivamente. Estos valores configuran el HyperLinkField de forma que todos los hipervínculos muestren el texto "Manage", pero cada vínculo pasa el valor de *username* adecuado a QueryString.

Después de agregar HyperLinkField a GridView, dedique un momento a ver la página `ManageUsers.aspx` a través de un explorador. Como se muestra en la figura 1, cada fila de GridView ahora incluye un vínculo "administrar". Vínculo "administrar" para que los puntos de Bruce se `UserInformation.aspx?user=Bruce`, mientras que el vínculo "administrar" de Dave apunta a `UserInformation.aspx?user=Dave`.

[![HyperLinkField agrega un](unlocking-and-approving-user-accounts-cs/_static/image2.png)](unlocking-and-approving-user-accounts-cs/_static/image1.png)

**Figura 1**: el HyperLinkField agrega un vínculo "administrar" para cada cuenta de usuario ([haga clic para ver la imagen de tamaño completo](unlocking-and-approving-user-accounts-cs/_static/image3.png))

Vamos a crear la interfaz de usuario y el código para la página `UserInformation.aspx` en un momento, pero primero vamos a hablar sobre cómo cambiar mediante programación los Estados bloqueado y aprobado de un usuario. La [clase`MembershipUser`](https://msdn.microsoft.com/library/system.web.security.membershipuser.aspx) tiene propiedades [`IsLockedOut`](https://msdn.microsoft.com/library/system.web.security.membershipuser.islockedout.aspx) y [`IsApproved`](https://msdn.microsoft.com/library/system.web.security.membershipuser.isapproved.aspx). La propiedad `IsLockedOut` es de solo lectura. No hay ningún mecanismo para bloquear un usuario mediante programación; para desbloquear un usuario, use el [método`UnlockUser`](https://msdn.microsoft.com/library/system.web.security.membershipuser.unlockuser.aspx)de la clase `MembershipUser`. La propiedad `IsApproved` es legible y grabable. Para guardar los cambios en esta propiedad, es necesario llamar al [método`UpdateUser`](https://msdn.microsoft.com/library/system.web.security.membership.updateuser.aspx)de la clase `Membership`, pasando el objeto `MembershipUser` modificado.

Dado que la propiedad `IsApproved` se puede leer y escribir, un control CheckBox es probablemente el mejor elemento de la interfaz de usuario para configurar esta propiedad. Sin embargo, una casilla no funcionará para la propiedad `IsLockedOut` porque un administrador no puede bloquear a un usuario, solo puede desbloquear a un usuario. Una interfaz de usuario adecuada para la propiedad `IsLockedOut` es un botón que, al hacer clic en él, desbloquea la cuenta de usuario. Este botón solo debe habilitarse si el usuario está bloqueado.

### <a name="creating-theuserinformationaspxpage"></a>Crear la página`UserInformation.aspx`

Ahora estamos listos para implementar la interfaz de usuario en `UserInformation.aspx`. Abra esta página y agregue los siguientes controles Web:

- Control de hipervínculo que, cuando se hace clic en él, devuelve el administrador a la página `ManageUsers.aspx`.
- Un control Web de etiqueta para mostrar el nombre del usuario seleccionado. Establezca el `ID` de la etiqueta en `UserNameLabel` y borre su propiedad `Text`.
- Control CheckBox denominado `IsApproved`. Establezca su propiedad `AutoPostBack` en `true`.
- Control de etiqueta para mostrar la última fecha de bloqueo del usuario. Asigne a esta etiqueta el nombre `LastLockedOutDateLabel` y borre su propiedad `Text`.
- Un botón para desbloquear al usuario. Asigne un nombre a este botón `UnlockUserButton` y establezca su propiedad `Text` en "Unlock user".
- Un control de etiqueta para mostrar mensajes de estado, como "el estado aprobado del usuario se ha actualizado". Asigne a este control el nombre `StatusMessage`, borre su propiedad `Text` y establezca su propiedad `CssClass` en `Important`. (La clase `Important` CSS se define en el archivo de hoja de estilos `Styles.css`; muestra el texto correspondiente en una fuente de color rojo grande).

Después de agregar estos controles, el Vista de diseño en Visual Studio debe ser similar a la captura de pantalla de la figura 2.

[![crear la interfaz de usuario para UserInformation. aspx](unlocking-and-approving-user-accounts-cs/_static/image5.png)](unlocking-and-approving-user-accounts-cs/_static/image4.png)

**Figura 2**: creación de la interfaz de usuario para `UserInformation.aspx` ([haga clic para ver la imagen de tamaño completo](unlocking-and-approving-user-accounts-cs/_static/image6.png))

Una vez completada la interfaz de usuario, la siguiente tarea consiste en establecer la casilla `IsApproved` y otros controles basados en la información del usuario seleccionado. Cree un controlador de eventos para el evento de `Load` de la página y agregue el código siguiente:

[!code-csharp[Main](unlocking-and-approving-user-accounts-cs/samples/sample1.cs)]

El código anterior se inicia asegurándose de que se trata de la primera visita a la página y no de un postback posterior. A continuación, lee el nombre de usuario pasado por el campo `user` QueryString y recupera información acerca de esa cuenta de usuario mediante el método `Membership.GetUser(username)`. Si no se ha proporcionado ningún nombre de usuario a través de la cadena de acceso, o si no se encuentra el usuario especificado, el administrador se devuelve a la página `ManageUsers.aspx`.

A continuación, se muestra el valor `UserName` del objeto `MembershipUser` en el `UserNameLabel` y se activa la casilla `IsApproved` basándose en el valor de la propiedad `IsApproved`.

La [propiedad`LastLockoutDate`](https://msdn.microsoft.com/library/system.web.security.membershipuser.lastlockoutdate.aspx) del objeto `MembershipUser` devuelve un valor `DateTime` que indica cuándo se bloqueó por última vez el usuario. Si nunca se ha bloqueado el usuario, el valor devuelto depende del proveedor de pertenencia. Cuando se crea una nueva cuenta, el `SqlMembershipProvider` establece el campo `LastLockoutDate` de la `aspnet_Membership` de la tabla en `1754-01-01 12:00:00 AM`. En el código anterior se muestra una cadena vacía en el `LastLockoutDateLabel` si la propiedad `LastLockoutDate` se produce antes del año 2000; de lo contrario, la parte de la fecha de la propiedad `LastLockoutDate` se muestra en la etiqueta. La propiedad `UnlockUserButton'` s `Enabled` está establecida en el estado de bloqueo del usuario, lo que significa que este botón solo estará habilitado si el usuario está bloqueado.

Dedique un momento a probar la página `UserInformation.aspx` a través de un explorador. Por supuesto, tendrá que iniciar en `ManageUsers.aspx` y seleccionar una cuenta de usuario para administrarla. Al llegar a `UserInformation.aspx`, tenga en cuenta que la casilla `IsApproved` solo se comprueba si se aprueba el usuario. Si el usuario se ha bloqueado alguna vez, se muestra la fecha de la última vez que se bloqueó. El botón Desbloquear usuario solo está habilitado si el usuario está bloqueado actualmente. Al activar o desactivar la casilla de `IsApproved` o al hacer clic en el botón Desbloquear usuario, se produce un postback, pero no se realiza ninguna modificación en la cuenta de usuario porque todavía hemos creado controladores de eventos para estos eventos.

Vuelva a Visual Studio y cree controladores de eventos para el evento `CheckedChanged` de la casilla de `IsApproved` y el evento `Click` del botón `UnlockUser`. En el controlador de eventos `CheckedChanged`, establezca la propiedad `IsApproved` del usuario en la propiedad `Checked` de la casilla y, a continuación, guarde los cambios a través de una llamada a `Membership.UpdateUser`. En el controlador de eventos `Click`, simplemente llame al método `UnlockUser` del objeto `MembershipUser`. En ambos controladores de eventos, muestre un mensaje adecuado en la etiqueta de `StatusMessage`.

[!code-csharp[Main](unlocking-and-approving-user-accounts-cs/samples/sample2.cs)]

### <a name="testing-theuserinformationaspxpage"></a>Probar la página`UserInformation.aspx`

Con estos controladores de eventos en su lugar, vuelva a visitar la página y sin aprobar un usuario. Como se muestra en la figura 3, debería ver un mensaje breve en la página que indica que la propiedad `IsApproved` del usuario se modificó correctamente.

[no se ha aprobado ![Chris](unlocking-and-approving-user-accounts-cs/_static/image8.png)](unlocking-and-approving-user-accounts-cs/_static/image7.png)

**Figura 3**: Carlos no aprobado ([haga clic para ver la imagen de tamaño completo](unlocking-and-approving-user-accounts-cs/_static/image9.png))

A continuación, cierre la sesión e intente iniciar sesión como el usuario cuya cuenta se acaba de aprobar. Dado que el usuario no está aprobado, no puede iniciar sesión. De forma predeterminada, el control de inicio de sesión muestra el mismo mensaje si el usuario no puede iniciar sesión, independientemente de la razón. Pero en el <a id="Tutorial6"> </a>tutorial [*validación de las credenciales de usuario en el almacén de usuarios de pertenencia*](../membership/validating-user-credentials-against-the-membership-user-store-cs.md) , hemos visto cómo mejorar el control de inicio de sesión para mostrar un mensaje más adecuado. Como se muestra en la figura 4, se muestra un mensaje en el que se explica que no puede iniciar sesión porque su cuenta aún no se ha aprobado.

[![Chris no puede iniciar sesión porque su cuenta no está aprobada](unlocking-and-approving-user-accounts-cs/_static/image11.png)](unlocking-and-approving-user-accounts-cs/_static/image10.png)

**Figura 4**: Chris no puede iniciar sesión porque su cuenta no está aprobada ([haga clic para ver la imagen de tamaño completo](unlocking-and-approving-user-accounts-cs/_static/image12.png))

Para probar la funcionalidad de bloqueo, intente iniciar sesión como un usuario aprobado, pero use una contraseña incorrecta. Repita este proceso el número de veces que sea necesario hasta que la cuenta del usuario se haya bloqueado. El control de inicio de sesión también se actualizó para mostrar un mensaje personalizado si se intenta iniciar sesión desde una cuenta bloqueada. Sabe que una cuenta se ha bloqueado una vez que empiece a ver el siguiente mensaje en la página de inicio de sesión: "su cuenta se ha bloqueado debido a que hay demasiados intentos de inicio de sesión no válidos. Póngase en contacto con el administrador para que se desbloquee la cuenta ".

Vuelva a la página `ManageUsers.aspx` y haga clic en el vínculo administrar del usuario bloqueado. Como se muestra en la figura 5, debería ver un valor en la `LastLockedOutDateLabel` el botón Desbloquear usuario debe estar habilitado. Haga clic en el botón Desbloquear usuario para desbloquear la cuenta de usuario. Una vez que haya desbloqueado el usuario, podrá iniciar sesión de nuevo.

[![David se ha bloqueado fuera del sistema](unlocking-and-approving-user-accounts-cs/_static/image14.png)](unlocking-and-approving-user-accounts-cs/_static/image13.png)

**Figura 5**: David está bloqueado en el sistema ([haga clic para ver la imagen de tamaño completo](unlocking-and-approving-user-accounts-cs/_static/image15.png))

## <a name="step-2-specifying-new-users-approved-status"></a>Paso 2: especificar el estado aprobado de los nuevos usuarios

El estado aprobado es útil en escenarios en los que desea realizar alguna acción antes de que un nuevo usuario pueda iniciar sesión y tener acceso a las características específicas del sitio. Por ejemplo, puede que esté ejecutando un sitio web privado en el que todas las páginas, excepto las páginas de inicio de sesión y suscripción, solo sean accesibles para los usuarios autenticados. Pero, ¿qué ocurre si un extraño llega a su sitio web, encuentra la página de registro y crea una cuenta? Para evitar que esto suceda, puede trasladar la página de registro a un `Administration` carpeta y requerir que un administrador cree manualmente cada cuenta. Como alternativa, puede permitir que cualquier persona se ponga en suscripción, pero prohibir el acceso al sitio hasta que un administrador apruebe la cuenta de usuario.

De forma predeterminada, el control CreateUserWizard aprueba nuevas cuentas. Puede configurar este comportamiento mediante la [propiedad`DisableCreatedUser`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.disablecreateduser.aspx)del control. Establezca esta propiedad en `true` para no aprobar nuevas cuentas de usuario.

> [!NOTE]
> De forma predeterminada, el control CreateUserWizard inicia sesión automáticamente en la nueva cuenta de usuario. Este comportamiento viene determinado por la [propiedad`LoginCreatedUser`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.logincreateduser.aspx)del control. Dado que los usuarios no aprobados no pueden iniciar sesión en el sitio, cuando `DisableCreatedUser` está `true` la nueva cuenta de usuario no inicia sesión en el sitio, independientemente del valor de la propiedad `LoginCreatedUser`.

Si va a crear nuevas cuentas de usuario mediante programación a través del método `Membership.CreateUser`, para crear una cuenta de usuario no aprobada, use una de las sobrecargas que acepten el valor de la propiedad `IsApproved` del usuario nuevo como parámetro de entrada.

## <a name="step-3-approving-users-by-verifying-their-email-address"></a>Paso 3: aprobación de los usuarios mediante la comprobación de su dirección de correo electrónico

Muchos sitios web que admiten cuentas de usuario no aprueban a los nuevos usuarios hasta que comprueban la dirección de correo electrónico proporcionada al registrarse. Este proceso de comprobación se usa normalmente para combatir bots, remitentes de correo no deseado y otros ne'er, ya que requiere una dirección de correo electrónico única y comprobada y agrega un paso adicional en el proceso de suscripción. Con este modelo, cuando un nuevo usuario se suscribe, se le envía un mensaje de correo electrónico que incluye un vínculo a una página de comprobación. Al visitar el vínculo, el usuario ha demostrado que ha recibido el correo electrónico y, por lo tanto, la dirección de correo electrónico proporcionada es válida. La página de comprobación es responsable de la aprobación del usuario. Esto puede ocurrir automáticamente, con lo que se aprueba cualquier usuario que llegue a esta página o solo después de que el usuario proporcione información adicional, como un [CAPTCHA](http://en.wikipedia.org/wiki/Captcha).

Para dar cabida a este flujo de trabajo, es necesario actualizar primero la página de creación de la cuenta para que los nuevos usuarios no estén aprobados. Abra la página `EnhancedCreateUserWizard.aspx` en la carpeta `Membership` y establezca la propiedad `DisableCreatedUser` del control CreateUserWizard en `true`.

A continuación, es necesario configurar el control CreateUserWizard para enviar un correo electrónico al nuevo usuario con instrucciones sobre cómo comprobar su cuenta. En concreto, incluiremos un vínculo en el correo electrónico a la página `Verification.aspx` (que todavía hemos creado), pasando el `UserId` del nuevo usuario a través de la cadena de mensajes. En la página `Verification.aspx` se buscará el usuario especificado y se marcarán como aprobados.

### <a name="sending-a-verification-email-to-new-users"></a>Envío de un correo electrónico de comprobación a nuevos usuarios

Para enviar un correo electrónico desde el control CreateUserWizard, configure la propiedad `MailDefinition` correctamente. Como se explicó en <a id="Tutorial13"> </a>el [tutorial anterior](recovering-and-changing-passwords-cs.md), los controles ChangePassword y PasswordRecovery incluyen una [`MailDefinition` propiedad](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.maildefinition.aspx) que funciona de la misma manera que el control CreateUserWizard.

> [!NOTE]
> Para usar la propiedad `MailDefinition` debe especificar las opciones de entrega de correo en `Web.config`. Para obtener más información, consulte [envío de correo electrónico en ASP.net](http://aspnet.4guysfromrolla.com/articles/072606-1.aspx).

Empiece por crear una nueva plantilla de correo electrónico denominada `CreateUserWizard.txt` en la carpeta `EmailTemplates`. Use el siguiente texto para la plantilla:

[!code-aspx[Main](unlocking-and-approving-user-accounts-cs/samples/sample3.aspx)]

Establezca la propiedad `MailDefinition'` s `BodyFileName` en "~/EmailTemplates/CreateUserWizard.txt" y su propiedad `Subject` en "Bienvenido a mi sitio Web. Active su cuenta ".

Tenga en cuenta que la plantilla de correo electrónico de `CreateUserWizard.txt` incluye un marcador de posición de `<%VerificationUrl%>`. Aquí es donde se colocará la dirección URL de la página `Verification.aspx`. CreateUserWizard reemplaza automáticamente los marcadores de posición `<%UserName%>` y `<%Password%>` por el nombre de usuario y la contraseña de la nueva cuenta, pero no hay ningún marcador de posición de `<%VerificationUrl%>` integrado. Necesitamos reemplazarlo manualmente por la dirección URL de comprobación adecuada.

Para ello, cree un controlador de eventos para el evento de [`SendingMail`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.sendingmail.aspx) de CreateUserWizard y agregue el código siguiente:

[!code-csharp[Main](unlocking-and-approving-user-accounts-cs/samples/sample4.cs)]

El evento `SendingMail` se desencadena después del evento `CreatedUser`, lo que significa que, en el momento en que se ha creado el controlador de eventos anterior, ya se ha creado la nueva cuenta de usuario. Podemos acceder al valor de `UserId` del nuevo usuario llamando al método `Membership.GetUser`, pasando el `UserName` especificado en el control CreateUserWizard. A continuación, se forma la dirección URL de comprobación. La instrucción `Request.Url.GetLeftPart(UriPartial.Authority)` devuelve la parte `http://yourserver.com` de la dirección URL; `Request.ApplicationPath` devuelve la ruta de acceso donde se encuentra la raíz de la aplicación. La dirección URL de comprobación se define como `Verification.aspx?ID=userId`. A continuación, estas dos cadenas se concatenan para formar la dirección URL completa. Por último, el cuerpo del mensaje de correo electrónico (`e.Message.Body`) tiene todas las apariciones de `<%VerificationUrl%>` reemplazadas por la dirección URL completa.

El efecto neto es que los nuevos usuarios no están aprobados, lo que significa que no pueden iniciar sesión en el sitio. Además, se envía automáticamente un correo electrónico con un vínculo a la dirección URL de comprobación (consulte la figura 6).

[![el nuevo usuario recibe un correo electrónico con un vínculo a la dirección URL de comprobación](unlocking-and-approving-user-accounts-cs/_static/image17.png)](unlocking-and-approving-user-accounts-cs/_static/image16.png)

**Figura 6**: el nuevo usuario recibe un correo electrónico con un vínculo a la dirección URL de comprobación ([haga clic para ver la imagen de tamaño completo](unlocking-and-approving-user-accounts-cs/_static/image18.png))

> [!NOTE]
> El paso CreateUserWizard predeterminado del control CreateUserWizard muestra un mensaje que informa al usuario de que se ha creado su cuenta y muestra el botón continuar. Al hacer clic en esta, se toma al usuario la dirección URL especificada por la propiedad `ContinueDestinationPageUrl` del control. CreateUserWizard en `EnhancedCreateUserWizard.aspx` está configurado para enviar nuevos usuarios al `~/Membership/AdditionalUserInfo.aspx`, que solicita al usuario su Hometown, la dirección URL de la Página principal y la firma. Dado que esta información solo la pueden agregar usuarios que han iniciado sesión, tiene sentido actualizar esta propiedad para devolver a los usuarios a la Página principal del sitio (`~/Default.aspx`). Además, la página `EnhancedCreateUserWizard.aspx` o el paso CreateUserWizard deben aumentarse para informar al usuario de que se ha enviado un correo electrónico de comprobación y su cuenta no se activará hasta que sigan las instrucciones de este correo electrónico. Dejo estas modificaciones como un ejercicio para el lector.

### <a name="creating-the-verification-page"></a>Crear la página de comprobación

Nuestra tarea final es crear la página `Verification.aspx`. Agregue esta página a la carpeta raíz, asociándolo a la página maestra de `Site.master`. Como hemos hecho con la mayoría de las páginas de contenido anteriores agregadas al sitio, quite el control de contenido que hace referencia a la `LoginContent` ContentPlaceHolder para que la página de contenido utilice el contenido predeterminado de la página maestra.

Agregue un control Web de etiqueta a la página `Verification.aspx`, establezca su `ID` en `StatusMessage` y borre su propiedad Text. A continuación, cree el controlador de eventos `Page_Load` y agregue el código siguiente:

[!code-csharp[Main](unlocking-and-approving-user-accounts-cs/samples/sample5.cs)]

La mayor parte del código anterior comprueba que el `UserId` proporcionado a través de QueryString existe, que es un valor de `Guid` válido y que hace referencia a una cuenta de usuario existente. Si todas estas comprobaciones son correctas, se aprueba la cuenta de usuario; de lo contrario, se muestra un mensaje de estado adecuado.

En la ilustración 7 se muestra la página `Verification.aspx` cuando se visita a través de un explorador.

[![la cuenta del usuario nuevo ya está aprobada](unlocking-and-approving-user-accounts-cs/_static/image20.png)](unlocking-and-approving-user-accounts-cs/_static/image19.png)

**Figura 7**: la cuenta del usuario nuevo ya está aprobada ([haga clic para ver la imagen de tamaño completo](unlocking-and-approving-user-accounts-cs/_static/image21.png))

## <a name="summary"></a>Resumen

Todas las cuentas de usuario de pertenencia tienen dos Estados que determinan si el usuario puede iniciar sesión en el sitio: `IsLockedOut` y `IsApproved`. Ambas propiedades deben estar `true` para que el usuario inicie sesión.

El estado de bloqueo del usuario se usa como medida de seguridad para reducir la probabilidad de que un hacker se interrumpa en un sitio mediante métodos de fuerza bruta. En concreto, se bloquea un usuario si hay un determinado número de intentos de inicio de sesión no válidos dentro de un período de tiempo determinado. Estos límites se pueden configurar a través de la configuración del proveedor de pertenencia en `Web.config`.

El estado aprobado se usa normalmente como un medio para impedir que los nuevos usuarios inicien sesión hasta que haya transcurrido alguna acción. Es posible que el sitio requiera que el administrador apruebe primero las cuentas nuevas o, como vimos en el paso 3, comprobando su dirección de correo electrónico.

¡ Feliz programación!

### <a name="about-the-author"></a>Acerca del autor

Scott Mitchell, autor de varios libros de ASP/ASP. NET y fundador de 4GuysFromRolla.com, ha estado trabajando con las tecnologías Web de Microsoft desde 1998. Scott funciona como consultor, profesor y redactor independiente. Su último libro se *[enseña a ASP.NET 2,0 en 24 horas](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)* . Se puede acceder a Scott en [mitchell@4guysfromrolla.com](mailto:mitchell@4guysfromrolla.com) o a través de su blog en [http://ScottOnWriting.NET](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Agradecimiento especial...

Muchos revisores útiles revisaron esta serie de tutoriales. ¿Está interesado en revisar los próximos artículos de MSDN? Si es así, coloque una línea en [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](recovering-and-changing-passwords-cs.md)
> [Siguiente](building-an-interface-to-select-one-user-account-from-many-vb.md)
