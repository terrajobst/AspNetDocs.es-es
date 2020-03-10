---
uid: web-forms/overview/older-versions-security/admin/recovering-and-changing-passwords-vb
title: Recuperar y cambiar contraseñas (VB) | Microsoft Docs
author: rick-anderson
description: ASP.NET incluye dos controles Web para ayudar a recuperar y cambiar las contraseñas. El control PasswordRecovery permite a un visitante recuperar el PA perdido...
ms.author: riande
ms.date: 04/01/2008
ms.assetid: f9adcb5d-6d70-4885-a3bf-ed95efb4da1a
msc.legacyurl: /web-forms/overview/older-versions-security/admin/recovering-and-changing-passwords-vb
msc.type: authoredcontent
ms.openlocfilehash: 0ed1df9455af94a86ce59ecc06c55846a4880596
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78510613"
---
# <a name="recovering-and-changing-passwords-vb"></a>Recuperar y cambiar las contraseñas (VB)

por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descargar código](https://download.microsoft.com/download/6/0/e/60e1bd94-e5f9-4d5a-a079-f23c98f4f67d/VB.13.zip) o [Descargar PDF](https://download.microsoft.com/download/6/0/e/60e1bd94-e5f9-4d5a-a079-f23c98f4f67d/aspnet_tutorial13_ChangingPasswords_vb.pdf)

> ASP.NET incluye dos controles Web para ayudar a recuperar y cambiar las contraseñas. El control PasswordRecovery permite a un visitante recuperar su contraseña perdida. El control ChangePassword permite al usuario actualizar su contraseña. Al igual que los demás controles Web relacionados con el inicio de sesión que hemos visto en esta serie de tutoriales, los controles PasswordRecovery y ChangePassword funcionan con el marco de pertenencia en segundo plano para restablecer o modificar las contraseñas de los usuarios.

## <a name="introduction"></a>Introducción

Entre los sitios web de mi banco, la compañía de la utilidad, la compañía telefónica, las cuentas de correo electrónico y los portales web personalizados, I, al igual que la mayoría de las personas, tienen docenas de contraseñas diferentes que recordar. Con tantas credenciales para memorizar estos días, no es raro que los usuarios olviden su contraseña. Para tener esto en cuenta, los sitios web que ofrecen cuentas de usuario deben incluir una manera de que un usuario pueda recuperar su contraseña. Normalmente, este proceso implica la generación de una nueva contraseña aleatoria y su correo electrónico a la dirección de correo electrónico del usuario en el archivo. Después de recibir la nueva contraseña, la mayoría de los usuarios vuelven al sitio y cambian su contraseña de la generada aleatoriamente a una más memorable.

ASP.NET incluye dos controles Web para ayudar a recuperar y cambiar las contraseñas. El control PasswordRecovery permite a un visitante recuperar su contraseña perdida. El control ChangePassword permite al usuario actualizar su contraseña. Al igual que los demás controles Web relacionados con el inicio de sesión que hemos visto en esta serie de tutoriales, los controles PasswordRecovery y ChangePassword funcionan con el marco de pertenencia en segundo plano para restablecer o modificar las contraseñas de los usuarios.

En este tutorial se examinará el uso de estos dos controles. También veremos cómo cambiar y restablecer la contraseña de un usuario mediante programación a través de los métodos `ChangePassword` y `ResetPassword` de la clase `MembershipUser`.

## <a name="step-1-helping-users-recover-lost-passwords"></a>Paso 1: ayudar a los usuarios a recuperar las contraseñas perdidas

Todos los sitios web que admiten cuentas de usuario deben proporcionar a los usuarios algún mecanismo para recuperar sus contraseñas olvidadas. La buena noticia es que la implementación de esta funcionalidad en ASP.NET es muy sencilla gracias al control Web PasswordRecovery. El control PasswordRecovery representa una interfaz que solicita al usuario su nombre de usuario y, si es necesario, la respuesta a su pregunta de seguridad. Después, envía por correo electrónico al usuario su contraseña.

> [!NOTE]
> Dado que los mensajes de correo electrónico se transmiten a través de la conexión en texto sin formato, hay riesgos de seguridad relacionados con el envío de la contraseña de un usuario por correo electrónico.

El control PasswordRecovery consta de tres vistas:

- **Nombre de usuario** : pide al visitante su nombre de usuario. Esta es la vista inicial.
- **Pregunta**: muestra el nombre de usuario y la pregunta de seguridad del usuario como texto, junto con un cuadro de texto para que el usuario escriba la respuesta a su pregunta de seguridad.
- **Correcto**: muestra un mensaje que informa al usuario de que se ha realizado su contraseña de correo electrónico.

Las vistas mostradas y las acciones realizadas por el control PasswordRecovery dependen de los siguientes valores de configuración de pertenencia:

- `RequiresQuestionAndAnswer`
- `EnablePasswordRetrieval`
- `EnablePasswordReset`

La configuración `RequiresQuestionAndAnswer` del marco de pertenencia indica si los usuarios deben especificar una pregunta y respuesta de seguridad al registrarse para una cuenta. Como se explicó en el <a id="_msoanchor_1"> </a>tutorial [*creación de cuentas de usuario*](../membership/creating-user-accounts-vb.md) , si `RequiresQuestionAndAnswer` es true (valor predeterminado), la interfaz de CreateUserWizard incluye controles de cuadro de texto para la pregunta y respuesta de seguridad del nuevo usuario. Si `RequiresQuestionAndAnswer` es false, no se recopila dicha información. Del mismo modo, si `RequiresQuestionAndAnswer` es true, el control PasswordRecovery muestra la vista de preguntas después de que el usuario haya escrito su nombre de usuario. la contraseña solo se recupera si el usuario escribe la respuesta de seguridad correcta. Sin embargo, si `RequiresQuestionAndAnswer` es false, el control PasswordRecovery se mueve directamente de la vista de nombre de usuario a la vista correcto.

Una vez que el usuario ha proporcionado su nombre de usuario o su nombre de usuario y respuesta de seguridad, si `RequiresQuestionAndAnswer` es true, el PasswordRecovery envía una contraseña al usuario. Si la opción `EnablePasswordRetrieval` está establecida en true, el usuario enviará por correo electrónico su contraseña actual. Si se establece en false y `EnablePasswordReset` está establecido en true, el control PasswordRecovery genera una nueva contraseña aleatoria para el usuario y le envía por correo electrónico esta nueva contraseña. Si tanto `EnablePasswordRetrieval` como `EnablePasswordReset` son false, el control PasswordRecovery produce una excepción.

> [!NOTE]
> Recuerde que el `SqlMembershipProvider` almacena las contraseñas de los usuarios en uno de estos tres formatos: borrar, con hash (valor predeterminado) o cifrado. El mecanismo de almacenamiento utilizado depende de la configuración de la pertenencia; la aplicación de demostración usa el formato de contraseña con hash. Al utilizar el formato de contraseña con hash, la opción `EnablePasswordRetrieval` debe establecerse en false, ya que el sistema no puede determinar la contraseña real del usuario a partir de la versión hash almacenada en la base de datos.

En la figura 1 se muestra cómo la configuración de pertenencia influye en la interfaz y el comportamiento de PasswordRecovery.

[![los controles RequiresQuestionAndAnswer, EnablePasswordRetrieval y EnablePasswordReset influyen en la apariencia y el comportamiento del control PasswordRecovery](recovering-and-changing-passwords-vb/_static/image2.png)](recovering-and-changing-passwords-vb/_static/image1.png)

**Figura 1**: el `RequiresQuestionAndAnswer`, el `EnablePasswordRetrieval`y el `EnablePasswordReset` influyen en la apariencia y el comportamiento del control PasswordRecovery ([haga clic para ver la imagen de tamaño completo](recovering-and-changing-passwords-vb/_static/image3.png))

> [!NOTE]
> En el <a id="_msoanchor_2"> </a>tutorial [*crear el esquema de pertenencia en SQL Server*](../membership/creating-the-membership-schema-in-sql-server-vb.md) se configuró el proveedor de pertenencia estableciendo `RequiresQuestionAndAnswer` en true, `EnablePasswordRetrieval` en false y `EnablePasswordReset` en true.

### <a name="using-the-passwordrecovery-control"></a>Usar el control PasswordRecovery

Echemos un vistazo al uso del control PasswordRecovery en una página de ASP.NET. Abra `RecoverPassword.aspx` y arrastre y coloque un control PasswordRecovery desde el cuadro de herramientas hasta el diseñador; establezca su `ID` en `RecoverPwd`. Al igual que los controles Web login y CreateUserWizard, las vistas del control PasswordRecovery representan una interfaz compuesta enriquecida que incluye etiquetas, cuadros de texto, botones y controles de validación. Puede personalizar la apariencia de las vistas a través de las propiedades de estilo del control o convirtiendo las vistas en plantillas. Dejo esto como un ejercicio para el lector interesado.

Cuando un usuario visita esta página, especifica su nombre de usuario y hace clic en el botón Enviar. Dado que hemos establecido la propiedad `RequiresQuestionAndAnswer` en true en la configuración de pertenencia, el control PasswordRecovery mostrará la vista de preguntas. Después de que el usuario escriba su respuesta de seguridad correcta y haga clic en enviar, el control PasswordRecovery actualizará la contraseña del usuario a una generada aleatoriamente y enviará por correo electrónico esta contraseña a la dirección de correo electrónico del archivo. Todo esto es posible sin tener que escribir una sola línea de código.

Antes de probar esta página, hay una parte final de configuración que tiende a: necesitamos especificar la configuración de entrega de correo en `Web.config`. El control PasswordRecovery se basa en esta configuración para enviar el correo electrónico.

La configuración de entrega de correo se especifica mediante el [elemento`<mailSettings>`](https://msdn.microsoft.com/library/w355a94k.aspx)del [elemento`<system.net>`](https://msdn.microsoft.com/library/6484zdc1.aspx). Utilice el [elemento`<smtp>`](https://msdn.microsoft.com/library/ms164240.aspx) para indicar el método de entrega y la dirección predeterminada de. El marcado siguiente configura las opciones de correo para usar un servidor SMTP de red denominado `smtp.example.com` en el puerto 25 y con las credenciales de nombre de usuario y contraseña de nombre de usuario y contraseña.

> [!NOTE]
> `<system.net>` es un elemento secundario del elemento de `<configuration>` raíz y un elemento del mismo nivel de `<system.web>`. Por lo tanto, no coloque el elemento `<system.net>` dentro del elemento `<system.web>`; en su lugar, colóquelo en el mismo nivel.

[!code-xml[Main](recovering-and-changing-passwords-vb/samples/sample1.xml)]

Además de usar un servidor SMTP en la red, también puede especificar un directorio de recogida en el que se deben depositar los mensajes de correo electrónico que se van a enviar.

Una vez que haya configurado la configuración de SMTP, visite la página `RecoverPassword.aspx` a través de un explorador. En primer lugar, intente escribir un nombre de usuario que no exista en el almacén de usuarios. Como se muestra en la figura 2, el control PasswordRecovery muestra un mensaje que indica que no se pudo tener acceso a la información del usuario. El texto del mensaje se puede personalizar mediante la [propiedad`UserNameFailureText`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.passwordrecovery.usernamefailuretext.aspx)del control.

[![se muestra un mensaje de error si se especifica un nombre de usuario no válido](recovering-and-changing-passwords-vb/_static/image5.png)](recovering-and-changing-passwords-vb/_static/image4.png)

**Figura 2**: se muestra un mensaje de error si se escribe un nombre de usuario no válido ([haga clic para ver la imagen de tamaño completo](recovering-and-changing-passwords-vb/_static/image6.png))

Ahora escriba un nombre de usuario. Use el nombre de usuario de una cuenta del sistema con una dirección de correo electrónico a la que pueda acceder y cuya respuesta de seguridad conozca. Después de escribir el nombre de usuario y hacer clic en enviar, el control PasswordRecovery muestra su vista de pregunta. Al igual que con la vista de nombre de usuario, si escribe una respuesta incorrecta, el control PasswordRecovery muestra un mensaje de error (vea la figura 3). Utilice la [propiedad`QuestionFailureText`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.passwordrecovery.questionfailuretext.aspx) para personalizar este mensaje de error.

[![se muestra un mensaje de error si el usuario escribe una respuesta de seguridad no válida](recovering-and-changing-passwords-vb/_static/image8.png)](recovering-and-changing-passwords-vb/_static/image7.png)

**Figura 3**: se muestra un mensaje de error si el usuario escribe una respuesta de seguridad no válida ([haga clic para ver la imagen de tamaño completo](recovering-and-changing-passwords-vb/_static/image9.png))

Por último, escriba la respuesta de seguridad correcta y haga clic en Submit (enviar). En segundo plano, el control PasswordRecovery genera una contraseña aleatoria, la asigna a la cuenta de usuario, envía un correo electrónico que informa al usuario de su nueva contraseña (vea la ilustración 4) y, a continuación, muestra la vista correcto.

[![se envía un correo electrónico al usuario con la nueva contraseña](recovering-and-changing-passwords-vb/_static/image11.png)](recovering-and-changing-passwords-vb/_static/image10.png)

**Figura 4**: al usuario se le envía un correo electrónico con la nueva contraseña ([haga clic para ver la imagen de tamaño completo](recovering-and-changing-passwords-vb/_static/image12.png))

### <a name="customizing-the-email"></a>Personalización del correo electrónico

El correo electrónico predeterminado enviado por el control PasswordRecovery es bastante mate (consulte la figura 4). El mensaje se envía desde la cuenta especificada en el `from` atributo del elemento de la `<smtp>` con la contraseña del firmante y el cuerpo del texto sin formato:

Vuelva al sitio e inicie sesión con la siguiente información.

Nombre de usuario: *nombreDeUsuario*

Contraseña: *contraseña*

Este mensaje se puede personalizar mediante programación a través de un controlador de eventos para el [evento`SendingMail`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.passwordrecovery.sendingmail.aspx)del control PasswordRecovery, o bien de forma declarativa a través de la [propiedad`MailDefinition`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.passwordrecovery.maildefinition.aspx). Vamos a explorar ambas opciones.

El evento `SendingMail` se desencadena justo antes de que se envíe el mensaje de correo electrónico y es nuestra última oportunidad de ajustar el mensaje de correo electrónico mediante programación. Cuando se genera este evento, se pasa al controlador de eventos un objeto de tipo [`MailMessageEventArgs`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.mailmessageeventargs.aspx), cuya propiedad `Message` contiene una referencia al correo electrónico que se va a enviar.

Cree un controlador de eventos para el evento `SendingMail` y agregue el código siguiente, que agrega mediante programación `webmaster@example.com` a la lista CC.

[!code-vb[Main](recovering-and-changing-passwords-vb/samples/sample2.vb)]

El mensaje de correo electrónico también puede configurarse mediante medios declarativos. La propiedad `MailDefinition` de PasswordRecovery es un objeto de tipo [`MailDefinition`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.maildefinition.aspx). La clase `MailDefinition` ofrece un host de propiedades relacionadas con el correo electrónico, como `From`, `CC`, `Priority`, `Subject`, `IsBodyHtml`, `BodyFileName`y otras. En el caso de los iniciadores, establezca la [propiedad`Subject`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.maildefinition.subject.aspx) en un valor más descriptivo que el utilizado de forma predeterminada (contraseña), como la contraseña se ha restablecido...

Para personalizar el cuerpo del mensaje de correo electrónico, es necesario crear un archivo de plantilla de correo electrónico independiente que contenga el contenido del cuerpo. Empiece por crear una nueva carpeta en el sitio web denominado `EmailTemplates`. Después, agregue un nuevo archivo de texto a esta carpeta denominado `PasswordRecovery.txt` y agregue el siguiente contenido:

[!code-aspx[Main](recovering-and-changing-passwords-vb/samples/sample3.aspx)]

Tenga en cuenta el uso de los marcadores de posición `<%UserName%>` y `<%Password%>`. El control PasswordRecovery reemplaza automáticamente estos dos marcadores de posición por el nombre de usuario del usuario y la contraseña recuperada antes de enviar el correo electrónico.

Por último, apunte la [propiedad`BodyFileName`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.maildefinition.bodyfilename.aspx) del `MailDefinition`a la plantilla de correo electrónico que acabamos de crear (`~/EmailTemplates/PasswordRecovery.txt`).

Después de realizar estos cambios, vuelva a visitar la página `RecoverPassword.aspx` y escriba el nombre de usuario y la respuesta de seguridad. Recibirá un mensaje de correo electrónico similar al que se muestra en la figura 5. Tenga en cuenta que `webmaster@example.com` es CC y que el asunto y el cuerpo se han actualizado.

[![se han actualizado el asunto, el cuerpo y la lista de CC](recovering-and-changing-passwords-vb/_static/image14.png)](recovering-and-changing-passwords-vb/_static/image13.png)

**Figura 5**: se han actualizado el asunto, el cuerpo y la lista CC ([haga clic para ver la imagen de tamaño completo](recovering-and-changing-passwords-vb/_static/image15.png))

Para enviar un conjunto de correo electrónico con formato HTML [`IsBodyHtml`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.maildefinition.isbodyhtml.aspx) a true (el valor predeterminado es false) y actualizar la plantilla de correo electrónico para incluir HTML.

La propiedad `MailDefinition` no es exclusiva de la clase PasswordRecovery. Como veremos en el paso 2, el control ChangePassword también ofrece una propiedad `MailDefinition`. Además, el control CreateUserWizard incluye una propiedad de este tipo que se puede configurar para enviar automáticamente un mensaje de correo electrónico de bienvenida a los nuevos usuarios.

> [!NOTE]
> Actualmente no hay ningún vínculo en el panel de navegación izquierdo para llegar a la página `RecoverPassword.aspx`. Un usuario solo le interesará visitar esta página si no pudo iniciar sesión correctamente en el sitio. Por lo tanto, actualice la página `Login.aspx` para incluir un vínculo a la página de `RecoverPassword.aspx`.

### <a name="programmatically-resetting-a-users-password"></a>Restablecer la contraseña de un usuario mediante programación

Al restablecer la contraseña de un usuario, el control PasswordRecovery llama al [método`ResetPassword`](https://msdn.microsoft.com/library/system.web.security.membershipuser.resetpassword.aspx)del objeto `MembershipUser`. Este método tiene dos sobrecargas:

- **[`ResetPassword`](https://msdn.microsoft.com/library/d94bdzz2.aspx)** : restablece la contraseña de un usuario. Utilice esta sobrecarga si `RequiresQuestionAndAnswer` es false.
- **[`ResetPassword(securityAnswer)`](https://msdn.microsoft.com/library/d90zte4w.aspx)** : restablece la contraseña de un usuario solo si el *securityAnswer* proporcionado es correcto. Utilice esta sobrecarga si `RequiresQuestionAndAnswer` es true.

Ambas sobrecargas devuelven la nueva contraseña generada de forma aleatoria.

Al igual que con los demás métodos del marco de pertenencia, el método `ResetPassword` delega en el proveedor configurado. El `SqlMembershipProvider` invoca el `aspnet_Membership_ResetPassword` procedimiento almacenado, pasando el nombre de usuario del usuario, la nueva contraseña y la respuesta de contraseña proporcionada, entre otros campos. El procedimiento almacenado garantiza que la respuesta de la contraseña coincide y, a continuación, actualiza la contraseña del usuario.

Un par de notas de implementación de bajo nivel:

- Un usuario bloqueado no puede restablecer su contraseña. Sin embargo, es posible que un usuario no aprobado. Analizaremos los Estados bloqueado y aprobado con más detalle en el <a id="_msoanchor_3"> </a>tutorial de [*desbloqueo y aprobación de cuentas de usuario*](unlocking-and-approving-user-accounts-vb.md) .
- Si la respuesta de la contraseña es incorrecta, se incrementa el número de intentos de respuesta de contraseña errónea del usuario. Si se produce un número especificado de intentos de respuesta de seguridad no válidos dentro de un período de tiempo especificado, el usuario se bloquea.

### <a name="a-word-on-how-the-random-passwords-are-generated"></a>Una palabra sobre cómo se generan las contraseñas aleatorias

Las contraseñas generadas de forma aleatoria que se muestran en los mensajes de correo electrónico en las figuras 4 y 5 se crean mediante el [método`GeneratePassword`](https://msdn.microsoft.com/library/system.web.security.membership.generatepassword.aspx)de la clase de pertenencia. Este método acepta dos parámetros de entrada de tipo entero: *length* y *numberOfNonAlphanumericCharacters* -y devuelve una cadena con una *longitud* de caracteres como mínimo, con al menos *numberOfNonAlphanumericCharacters* número de caracteres no alfanuméricos. Cuando se llama a este método desde las clases de pertenencia o los controles Web relacionados con el inicio de sesión, los valores de estos dos parámetros se determinan mediante las propiedades `MinRequiredPasswordLength` y `MinRequiredNonalphanumericCharacters` de la configuración de pertenencia, que se establecen en 7 y 1, respectivamente.

El método `GeneratePassword` usa un generador de números aleatorios criptográficamente seguro para asegurarse de que no hay ninguna diferencia en lo que se seleccionan los caracteres aleatorios. Además, `GeneratePassword` es `Public`, lo que significa que se puede usar directamente desde la aplicación ASP.NET si necesita generar cadenas o contraseñas aleatorias.

> [!NOTE]
> La clase `SqlMembershipProvider` siempre genera una contraseña aleatoria con una longitud mínima de 14 caracteres, por lo que si `MinRequiredPasswordLength` es inferior a 14, se omite su valor.

## <a name="step-2-changing-passwords"></a>Paso 2: cambiar las contraseñas

Las contraseñas generadas de forma aleatoria son difíciles de recordar. Tenga en cuenta la contraseña que se muestra en la figura 4: `WWGUZv(f2yM:Bd`. Intente confirmarlo en la memoria. No es necesario decir que, una vez que se envía a un usuario una contraseña generada aleatoriamente de este tipo, querrá cambiar la contraseña por algo más memorable.

Use el control ChangePassword para crear una interfaz para que un usuario cambie su contraseña. Al igual que el control PasswordRecovery, el control ChangePassword está formado por dos vistas: cambiar contraseña y correcto. La vista cambiar contraseña solicita al usuario sus contraseñas antiguas y nuevas. Tras proporcionar la contraseña antigua correcta y una nueva contraseña que cumpla los requisitos de longitud mínima y caracteres no alfanuméricos, el control ChangePassword actualiza la contraseña del usuario y muestra la vista correcto.

> [!NOTE]
> El control ChangePassword modifica la contraseña del usuario mediante la invocación del [método`ChangePassword`](https://msdn.microsoft.com/library/system.web.security.membershipuser.changepassword.aspx)del objeto `MembershipUser`. El método ChangePassword acepta dos `String` parámetros de entrada: *oldPassword* y *NuevaContraseña*, y actualiza la cuenta del usuario con *NuevaContraseña*, suponiendo que el *oldPassword* proporcionado sea correcto.

Abra la página `ChangePassword.aspx` y agregue un control ChangePassword a la página, asignándole un nombre `ChangePwd`. En este momento, el Vista de diseño debe mostrar la vista cambiar contraseña (consulte la figura 6). Al igual que con el control PasswordRecovery, puede alternar entre las vistas a través de la etiqueta inteligente del control. Además, estas vistas de vista se pueden personalizar a través de las propiedades de estilo asordenas o mediante su conversión en una plantilla.

[![agregar un control ChangePassword a la página](recovering-and-changing-passwords-vb/_static/image17.png)](recovering-and-changing-passwords-vb/_static/image16.png)

**Figura 6**: agregar un control ChangePassword a la página ([haga clic para ver la imagen de tamaño completo](recovering-and-changing-passwords-vb/_static/image18.png))

El control ChangePassword puede actualizar la contraseña del usuario que ha iniciado sesión actualmente *o* la contraseña de otro usuario especificado. Como se muestra en la figura 6, la vista cambiar contraseña predeterminada representa solo tres controles de cuadro de texto: uno para la contraseña anterior y otro para la nueva contraseña. Esta interfaz predeterminada se utiliza para actualizar la contraseña del usuario que ha iniciado sesión actualmente.

Para usar el control ChangePassword para actualizar la contraseña de otro usuario, establezca la [propiedad`DisplayUserName`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.changepassword.displayusername.aspx) del control en true. Al hacerlo, se agrega un cuarto cuadro de texto a la página y se solicita el nombre del usuario cuya contraseña se va a cambiar.

Establecer `DisplayUserName` en true es útil si desea permitir que un usuario con sesión iniciada cambie su contraseña sin tener que iniciar sesión. Personalmente, creo que no hay ningún problema al requerir que un usuario inicie sesión antes de permitirle cambiar su contraseña. Por lo tanto, deje `DisplayUserName` establecido en false (su valor predeterminado). Sin embargo, al tomar esta decisión, básicamente nos Esponemos que los usuarios anónimos no llegarán a esta página. Actualice las reglas de autorización de URL del sitio para impedir que los usuarios anónimos visiten `ChangePassword.aspx`. Si necesita actualizar la memoria en la sintaxis de la regla de autorización de URL, consulte el <a id="_msoanchor_4"> </a>tutorial de [*autorización basada*](../membership/user-based-authorization-vb.md) en el usuario.

> [!NOTE]
> Puede parecer que la propiedad `DisplayUserName` es útil para permitir que los administradores cambien las contraseñas de otros usuarios. Sin embargo, incluso cuando `DisplayUserName` está establecido en true, se debe conocer y escribir la contraseña antigua correcta. Hablaremos sobre las técnicas para permitir que los administradores cambien las contraseñas de los usuarios en el paso 3.

Visite la página `ChangePassword.aspx` a través de un explorador y cambie la contraseña. Tenga en cuenta que se muestra un mensaje de error si escribe una nueva contraseña que no cumple los requisitos de longitud de la contraseña y caracteres no alfanuméricos especificados en la configuración de pertenencia (consulte la figura 7).

[![agregar un control ChangePassword a la página](recovering-and-changing-passwords-vb/_static/image20.png)](recovering-and-changing-passwords-vb/_static/image19.png)

**Figura 7**: agregar un control ChangePassword a la página ([haga clic para ver la imagen de tamaño completo](recovering-and-changing-passwords-vb/_static/image21.png))

Al escribir la contraseña anterior correcta y una contraseña nueva válida, se cambia la contraseña del usuario que ha iniciado sesión y se muestra la vista correcto.

### <a name="sending-a-confirmation-email"></a>Envío de un correo electrónico de confirmación

De forma predeterminada, el control ChangePassword no envía un mensaje de correo electrónico al usuario cuya contraseña se acaba de actualizar. Si desea enviar un correo electrónico, solo tiene que configurar la propiedad `MailDefinition` del control. Vamos a configurar el control ChangePassword para que al usuario se le envíe un correo electrónico con formato HTML que contenga la nueva contraseña.

Empiece por crear un nuevo archivo en la carpeta `EmailTemplates` denominada `ChangePassword.htm`. Agregue el marcado siguiente:

[!code-html[Main](recovering-and-changing-passwords-vb/samples/sample4.html)]

A continuación, establezca las propiedades `BodyFileName`, `IsBodyHtml`y `Subject` de la propiedad `MailDefinition` del control ChangePassword en ~/EmailTemplates/ChangePassword.htm, true y la contraseña ha cambiado! respectivamente.

Después de realizar estos cambios, vuelva a visitar la página y cambie la contraseña de nuevo. Esta vez, el control ChangePassword envía un correo electrónico personalizado con formato HTML a la dirección de correo electrónico del usuario en el archivo (vea la figura 8).

[![un mensaje de correo electrónico informa al usuario de que ha cambiado su contraseña](recovering-and-changing-passwords-vb/_static/image23.png)](recovering-and-changing-passwords-vb/_static/image22.png)

**Figura 8**: un mensaje de correo electrónico informa al usuario de que la contraseña ha cambiado ([haga clic para ver la imagen de tamaño completo](recovering-and-changing-passwords-vb/_static/image24.png))

## <a name="step-3-allowing-administrators-to-change-users-passwords"></a>Paso 3: permitir que los administradores cambien las contraseñas de los usuarios

Una característica común de las aplicaciones que admiten cuentas de usuario es la posibilidad de que un usuario administrativo cambie las contraseñas de otros usuarios. A veces, esta funcionalidad es necesaria porque el sistema carece de la capacidad para que los usuarios cambien sus contraseñas. En tal caso, la única manera de que un usuario recupere su contraseña olvidada es que el administrador les asigne una nueva contraseña. Sin embargo, con los controles PasswordRecovery y ChangePassword, no es necesario que los usuarios administrativos tengan que cambiar las contraseñas de los usuarios, ya que los usuarios pueden hacerlo.

Pero ¿qué ocurre si el cliente insiste en que los usuarios administrativos deben poder cambiar las contraseñas de otros usuarios? Desafortunadamente, agregar esta funcionalidad puede ser un poco de trabajo. Para cambiar la contraseña de un usuario, se debe proporcionar la contraseña antigua y la nueva en el método de `ChangePassword` del objeto `MembershipUser`, pero un administrador no debe tener que conocer la contraseña de un usuario para modificarla.

Una solución alternativa es restablecer primero la contraseña del usuario y, a continuación, cambiarla a la nueva contraseña mediante código como el siguiente:

[!code-vb[Main](recovering-and-changing-passwords-vb/samples/sample5.vb)]

Este código se inicia recuperando información sobre el *nombre de usuario*, que es el usuario cuya contraseña el administrador desea cambiar. A continuación, se invoca el método de `ResetPassword`, que asigna y al usuario una nueva contraseña aleatoria. El método devuelve esta contraseña generada aleatoriamente y se almacena en la variable `resetPwd`. Ahora que sabemos la contraseña del usuario, podemos cambiarla a través de una llamada a `ChangePassword`.

El problema es que este código solo funciona si la configuración del sistema de pertenencia está establecida de modo que `RequiresQuestionAndAnswer` es false. Si `RequiresQuestionAndAnswer` es true, como sucede con nuestra aplicación, el método de `ResetPassword` debe pasar la respuesta de seguridad, de lo contrario, producirá una excepción.

Si el marco de pertenencia está configurado para requerir una pregunta y respuesta de seguridad, y el cliente insiste en que los administradores puedan cambiar las contraseñas de los usuarios, tiene tres opciones:

- Lance sus manos en el aire e indique a su cliente que esto es solo una cosa que no se puede hacer.
- Establezca `RequiresQuestionAndAnswer` en false. Esto da como resultado una aplicación menos segura. Imagine que un usuario de fraudulento ha obtenido acceso a la bandeja de entrada de correo electrónico de otro usuario. Es posible que el usuario en peligro abandone su mesa de trabajo para comer y no bloquee su estación de trabajo, o quizás que haya tenido acceso a su correo electrónico desde un terminal público y no lo cierre. En cualquier caso, el usuario de fraudulento puede visitar la página `RecoverPassword.aspx` y escribir el nombre de usuario del usuario. Después, el sistema enviará por correo electrónico la contraseña recuperada sin pedir la respuesta de seguridad.
- Omita la capa de abstracción creada por el marco de pertenencia y trabaje directamente con la base de datos de SQL Server. El esquema de pertenencia incluye un procedimiento almacenado denominado `aspnet_Membership_SetPassword` que establece la contraseña de un usuario y no requiere la respuesta de seguridad ni la contraseña anterior para realizar su tarea.

Ninguna de estas opciones es especialmente atractiva, sino que es el aspecto de la vida de un desarrollador a veces.

He ido implementando el tercer enfoque, escribiendo código que omite las clases `Membership` y `MembershipUser` y funciona directamente con la base de datos `SecurityTutorials`.

> [!NOTE]
> Al trabajar directamente con la base de datos, se daña la encapsulación proporcionada por el marco de pertenencia. Esta decisión nos vincula al `SqlMembershipProvider`, lo que permite que nuestro código sea menos portátil. Además, es posible que este código no funcione según lo previsto en futuras versiones de ASP.NET si cambia el esquema de pertenencia. Este enfoque es una solución alternativa y, al igual que la mayoría de las soluciones alternativas, no es un ejemplo de procedimientos recomendados.

El código tiene algunos bits poco atractivos y es bastante largo. Por lo tanto, no deseo abarrotar este tutorial con un examen exhaustivo de él. Si está interesado en obtener más información, descargue el código de este tutorial y visite la página `~/Administration/ManageUsers.aspx`. En esta página, que hemos creado en <a id="_msoanchor_5"> </a>el [tutorial anterior](building-an-interface-to-select-one-user-account-from-many-vb.md), se muestra una lista de cada usuario. He actualizado GridView para incluir un vínculo a la página `UserInformation.aspx`, pasando el nombre de usuario del usuario seleccionado a través de QueryString. En la página `UserInformation.aspx` se muestra información sobre el usuario seleccionado y los cuadros de texto para cambiar su contraseña (consulte la figura 9).

Después de escribir la nueva contraseña, confirmarla en el segundo cuadro de texto y hacer clic en el botón actualizar usuario, se abre un postback y se invoca el `aspnet_Membership_SetPassword` procedimiento almacenado, actualizando la contraseña del usuario. Se recomienda que los lectores interesados en esta funcionalidad estén más familiarizados con el código y traten de extender la funcionalidad para incluir el envío de un correo electrónico al usuario cuya contraseña se ha cambiado.

[![un administrador puede cambiar la contraseña de un usuario](recovering-and-changing-passwords-vb/_static/image26.png)](recovering-and-changing-passwords-vb/_static/image25.png)

**Figura 9**: un administrador puede cambiar la contraseña de un usuario ([haga clic para ver la imagen de tamaño completo](recovering-and-changing-passwords-vb/_static/image27.png))

> [!NOTE]
> La página `UserInformation.aspx` actualmente solo funciona si el marco de pertenencia está configurado para almacenar las contraseñas con un formato sin cifrar o con el algoritmo hash. Falta el código para cifrar la nueva contraseña, aunque se le ha invitado a agregar esta funcionalidad. La forma en que recomiendo agregar el código necesario es usar un descompilador como [reflector](http://www.aisto.com/roeder/dotnet/) para examinar el código fuente de los métodos del .NET Framework; Empiece examinando el método de `ChangePassword` de la clase `SqlMembershipProvider`. Esta es la técnica que se usa para escribir el código para crear un hash de la contraseña.

## <a name="summary"></a>Resumen

ASP.NET ofrece dos controles para ayudar a los usuarios a administrar su contraseña. El control PasswordRecovery es útil para quienes han olvidado sus contraseñas. Dependiendo de la configuración del marco de pertenencia, el usuario puede enviar por correo electrónico su contraseña existente o una nueva contraseña generada de forma aleatoria. El control ChangePassword permite a un usuario actualizar su contraseña.

Al igual que los controles login y CreateUserWizard, los controles PasswordRecovery y ChangePassword representan una interfaz de usuario enriquecida sin tener que escribir una o una marca de código declarativo. Si la interfaz de usuario predeterminada no satisface sus necesidades, puede personalizarla a través de una variedad de propiedades de estilo. Como alternativa, las interfaces de los controles se pueden convertir en plantillas para un grado de control aún más preciso. En segundo plano, estos controles usan la API de pertenencia, invocando los métodos `ResetPassword` y `ChangePassword` del objeto `MembershipUser`.

¡ Feliz programación!

### <a name="further-reading"></a>Información adicional

Para obtener más información sobre los temas descritos en este tutorial, consulte los siguientes recursos:

- [Inicios rápidos del control ChangePassword](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/ctrlref/login/changepassword.aspx)
- [Inicios rápidos del control PasswordRecovery](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/ctrlref/login/passwordrecovery.aspx)
- [Envío de correo electrónico en ASP.NET](http://aspnet.4guysfromrolla.com/articles/072606-1.aspx)
- [Preguntas más frecuentes sobre `System.Net.Mail`](http://www.systemnetmail.com/)

### <a name="about-the-author"></a>Acerca del autor

Scott Mitchell, autor de varios libros de ASP/ASP. NET y fundador de 4GuysFromRolla.com, ha estado trabajando con las tecnologías Web de Microsoft desde 1998. Scott funciona como consultor, profesor y redactor independiente. Su último libro se *[enseña a ASP.NET 2,0 en 24 horas](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)* . Se puede acceder a Scott en [mitchell@4guysfromrolla.com](mailto:mitchell@4guysfromrolla.com) o a través de su blog en [http://ScottOnWriting.NET](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Agradecimiento especial a

Muchos revisores útiles revisaron esta serie de tutoriales. Los revisores responsables de este tutorial incluyen Michael Emmings y este Banerjee. ¿Está interesado en revisar los próximos artículos de MSDN? Si es así, coloque una línea en [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](building-an-interface-to-select-one-user-account-from-many-vb.md)
> [Siguiente](unlocking-and-approving-user-accounts-vb.md)
