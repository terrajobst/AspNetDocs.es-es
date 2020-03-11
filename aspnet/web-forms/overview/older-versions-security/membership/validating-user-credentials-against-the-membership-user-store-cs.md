---
uid: web-forms/overview/older-versions-security/membership/validating-user-credentials-against-the-membership-user-store-cs
title: Validación de las credenciales de usuario en elC#almacén de usuarios de pertenencia () | Microsoft Docs
author: rick-anderson
description: En este tutorial, veremos cómo validar las credenciales de un usuario en el almacén de usuarios de pertenencia mediante los medios de programación y el control de inicio de sesión....
ms.author: riande
ms.date: 01/18/2008
ms.assetid: 61aa4e08-aa81-4aeb-8ebe-19ba7a65e04c
msc.legacyurl: /web-forms/overview/older-versions-security/membership/validating-user-credentials-against-the-membership-user-store-cs
msc.type: authoredcontent
ms.openlocfilehash: aaf6df6f52253ef0f7369a7e77211b6786b97db1
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78420295"
---
# <a name="validating-user-credentials-against-the-membership-user-store-c"></a>Validar las credenciales de usuario en el almacén de usuarios de pertenencia (C#)

por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descargar código](https://download.microsoft.com/download/3/f/5/3f5a8605-c526-4b34-b3fd-a34167117633/ASPNET_Security_Tutorial_06_CS.zip) o [Descargar PDF](https://download.microsoft.com/download/3/f/5/3f5a8605-c526-4b34-b3fd-a34167117633/aspnet_tutorial06_LoggingIn_cs.pdf)

> En este tutorial, examinaremos cómo validar las credenciales de un usuario en el almacén de usuarios de pertenencia mediante el uso de los medios de programación y el control de inicio de sesión. También veremos cómo personalizar la apariencia y el comportamiento del control de inicio de sesión.

## <a name="introduction"></a>Introducción

En el <a id="Tutorial05"> </a> [tutorial anterior](creating-user-accounts-cs.md) , hemos visto cómo crear una nueva cuenta de usuario en el marco de pertenencia. En primer lugar, hemos examinado la creación de cuentas de usuario mediante programación a través del método `CreateUser` de la clase `Membership` y, a continuación, hemos examinado mediante el control Web CreateUserWizard. Sin embargo, la página de inicio de sesión valida actualmente las credenciales proporcionadas en una lista codificada de forma rígida de pares de nombre de usuario y contraseña. Necesitamos actualizar la lógica de la página de inicio de sesión para validar las credenciales en el almacén de usuario del marco de pertenencia.

De forma muy parecida a la creación de cuentas de usuario, las credenciales se pueden validar mediante programación o mediante declaración. La API de pertenencia incluye un método para validar mediante programación las credenciales de un usuario con el almacén del usuario. Y ASP.NET se distribuyen con el control Web de inicio de sesión, que representa una interfaz de usuario con cuadros de texto para el nombre de usuario y la contraseña, y un botón para iniciar sesión.

En este tutorial, examinaremos cómo validar las credenciales de un usuario en el almacén de usuarios de pertenencia mediante el uso de los medios de programación y el control de inicio de sesión. También veremos cómo personalizar la apariencia y el comportamiento del control de inicio de sesión. Comencemos.

## <a name="step-1-validating-credentials-against-the-membership-user-store"></a>Paso 1: validación de credenciales en el almacén de usuarios de pertenencia

En el caso de los sitios web que usan la autenticación de formularios, un usuario inicia sesión en el sitio Web visitando una página de inicio de sesión y especifica sus credenciales. Después, estas credenciales se comparan con el almacén del usuario. Si son válidos, se concede al usuario un vale de autenticación de formularios, que es un token de seguridad que indica la identidad y la autenticidad del visitante.

Para validar un usuario con el marco de pertenencia, use el [método`ValidateUser`](https://msdn.microsoft.com/library/system.web.security.membership.validateuser.aspx)de la clase `Membership`. El método `ValidateUser` toma dos parámetros de entrada: *`username`* y *`password`* -y devuelve un valor booleano que indica si las credenciales son válidas. Al igual que con el método `CreateUser` que examinamos en el tutorial anterior, el método `ValidateUser` delega la validación real en el proveedor de pertenencia configurado.

El `SqlMembershipProvider` valida las credenciales proporcionadas obteniendo la contraseña del usuario especificado mediante el procedimiento almacenado `aspnet_Membership_GetPasswordWithFormat`. Recuerde que el `SqlMembershipProvider` almacena las contraseñas de los usuarios con uno de estos tres formatos: Clear, encrypted o Hashed. El `aspnet_Membership_GetPasswordWithFormat` procedimiento almacenado devuelve la contraseña en su formato sin procesar. En el caso de las contraseñas cifradas o con hash, el `SqlMembershipProvider` transforma el *`password`* valor pasado al método `ValidateUser` en su estado cifrado o hash equivalente y, a continuación, lo compara con lo que se devolvió en la base de datos. Si la contraseña almacenada en la base de datos coincide con la especificada por el usuario, las credenciales son válidas.

Vamos a actualizar nuestra página de inicio de sesión (~/`Login.aspx`) para validar las credenciales proporcionadas en el almacén de usuario del marco de pertenencia. Esta página de inicio de sesión se creó <a id="Tutorial02"> </a>de nuevo en el tutorial Introducción a la [*autenticación de formularios*](../introduction/an-overview-of-forms-authentication-cs.md) , creación de una interfaz con dos cuadros de texto para el nombre de usuario y contraseña, una casilla recordarme y un botón de inicio de sesión (consulte la figura 1). El código valida las credenciales especificadas en una lista codificada de forma rígida de pares de nombre de usuario y contraseña (Scott/contraseña, Jisun/contraseña y Sam/contraseña). En el <a id="Tutorial03"> </a>tutorial configuración de la [*autenticación de formularios y temas avanzados*](../introduction/forms-authentication-configuration-and-advanced-topics-cs.md) , hemos actualizado el código de la página de inicio de sesión para almacenar información adicional en la propiedad `UserData` del vale de autenticación de formularios.

[![la interfaz de la página de inicio de sesión incluye dos cuadros de texto, un control CheckBoxList y un botón](validating-user-credentials-against-the-membership-user-store-cs/_static/image2.png)](validating-user-credentials-against-the-membership-user-store-cs/_static/image1.png)

**Figura 1**: la interfaz de la página de inicio de sesión incluye dos cuadros de texto, un CheckBoxList y un botón ([haga clic para ver la imagen de tamaño completo](validating-user-credentials-against-the-membership-user-store-cs/_static/image3.png))

La interfaz de usuario de la página de inicio de sesión puede permanecer sin cambios, pero es necesario reemplazar el controlador de eventos `Click` del botón de inicio de sesión por código que valida al usuario en el almacén de usuario del marco de pertenencia. Actualice el controlador de eventos para que el código aparezca como sigue:

[!code-csharp[Main](validating-user-credentials-against-the-membership-user-store-cs/samples/sample1.cs)]

Este código es muy sencillo. Comencemos llamando al método `Membership.ValidateUser`, pasando el nombre de usuario y la contraseña proporcionados. Si ese método devuelve true, el usuario inicia sesión en el sitio mediante el método RedirectFromLoginPage de la clase `FormsAuthentication`. (Como hemos comentado en <a id="Tutorial2"> </a>el tutorial [*de introducción a la autenticación de formularios*](../introduction/an-overview-of-forms-authentication-cs.md) , el `FormsAuthentication.RedirectFromLoginPage` crea el vale de autenticación de formularios y, a continuación, redirige al usuario a la página adecuada). Sin embargo, si las credenciales no son válidas, se muestra la etiqueta `InvalidCredentialsMessage`, lo que informa al usuario de que su nombre de usuario o contraseña eran incorrectos.

Así de simple.

Para comprobar que la página de inicio de sesión funciona según lo previsto, intente iniciar sesión con una de las cuentas de usuario que creó en el tutorial anterior. O bien, si aún no ha creado una cuenta, continúe y cree una en la página `~/Membership/CreatingUserAccounts.aspx`.

> [!NOTE]
> Cuando el usuario escribe sus credenciales y envía el formulario de la página de inicio de sesión, las credenciales, incluida su contraseña, se transmiten a través de Internet al servidor Web en *texto sin formato*. Esto significa que cualquier pirata informático que examine el tráfico de red puede ver el nombre de usuario y la contraseña. Para evitar esto, es esencial cifrar el tráfico de red mediante el uso de [capas de sockets seguros (SSL)](http://en.wikipedia.org/wiki/Secure_Sockets_Layer). Esto garantizará que las credenciales (así como el marcado HTML de la página completa) se cifren desde el momento en que salen del explorador hasta que las reciba el servidor Web.

### <a name="how-the-membership-framework-handles-invalid-login-attempts"></a>Cómo controla el marco de pertenencia los intentos de inicio de sesión no válidos

Cuando un visitante llega a la página de inicio de sesión y envía sus credenciales, el explorador realiza una solicitud HTTP a la página de inicio de sesión. Si las credenciales son válidas, la respuesta HTTP incluye el vale de autenticación en una cookie. Por lo tanto, un pirata informático que intente interrumpir su sitio podría crear un programa que envíe de forma exhaustiva las solicitudes HTTP a la página de inicio de sesión con un nombre de usuario válido y una estimación en la contraseña. Si la suposición de la contraseña es correcta, la página de inicio de sesión devolverá la cookie de vale de autenticación, momento en el que el programa sabe que tiene stumbled en un par de nombre de usuario/contraseña válido. Por fuerza bruta, este tipo de programa podría ser capaz de caso en la contraseña de un usuario, en especial si la contraseña es débil.

Para evitar estos ataques por fuerza bruta, el marco de pertenencia bloquea a un usuario si hay un cierto número de intentos de inicio de sesión incorrectos en un período de tiempo determinado. Los parámetros exactos se pueden configurar mediante las dos siguientes opciones de configuración del proveedor de pertenencia:

- `maxInvalidPasswordAttempts`: especifica el número de intentos de contraseña no válidos permitidos para el usuario durante el período de tiempo antes de que se bloquee la cuenta. El valor predeterminado es 5.
- `passwordAttemptWindow`: indica el período de tiempo en minutos durante el cual el número especificado de intentos de inicio de sesión no válidos hará que se bloquee la cuenta. El valor predeterminado es 10.

Si un usuario se ha bloqueado, no podrá iniciar sesión hasta que un administrador desbloquee su cuenta. Cuando se bloquea un usuario, el método de `ValidateUser` *siempre* devolverá `false`, incluso si se proporcionan credenciales válidas. Aunque este comportamiento reduce la probabilidad de que un pirata informático entre en el sitio a través de métodos de fuerza bruta, puede acabar bloqueando un usuario válido que simplemente olvidó su contraseña o que tiene un bloqueo de mayúsculas o minúsculas en un día de escritura incorrecto.

Desafortunadamente, no hay ninguna herramienta integrada para desbloquear una cuenta de usuario. Para desbloquear una cuenta, puede modificar la base de datos directamente: cambie el campo `IsLockedOut` en la tabla de `aspnet_Membership` de la cuenta de usuario correspondiente, o cree una interfaz basada en Web que muestre las cuentas bloqueadas con opciones para desbloquearlas. Examinaremos la creación de interfaces administrativas para llevar a cabo tareas comunes relacionadas con la cuenta de usuario y los roles en un tutorial futuro.

> [!NOTE]
> Uno de los inconvenientes del método `ValidateUser` es que cuando las credenciales proporcionadas no son válidas, no proporciona ninguna explicación de por qué. Es posible que las credenciales no sean válidas porque no hay ningún par de nombre de usuario/contraseña coincidente en el almacén del usuario, o porque el usuario aún no se ha aprobado o porque el usuario se ha bloqueado. En el paso 4, veremos cómo mostrar un mensaje más detallado al usuario cuando se produce un error en el intento de inicio de sesión.

## <a name="step-2-collecting-credentials-through-the-login-web-control"></a>Paso 2: recopilación de credenciales a través del control Web de inicio de sesión

El [control Web de inicio de sesión](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.aspx) representa una interfaz de usuario predeterminada muy similar a la que se ha <a id="SKM5"> </a>creado en el tutorial Introducción a la [*autenticación de formularios*](../introduction/an-overview-of-forms-authentication-cs.md) . El uso del control de inicio de sesión nos ahorra el trabajo de tener que crear la interfaz para recopilar las credenciales del visitante. Además, el control de inicio de sesión inicia sesión automáticamente en el usuario (suponiendo que las credenciales enviadas son válidas), lo que nos evita tener que escribir código.

Vamos a actualizar `Login.aspx`, reemplazando la interfaz y el código creados manualmente por un control de inicio de sesión. Comience quitando el marcado y el código existentes en `Login.aspx`. Puede eliminarlo de forma inadecuada o simplemente comentarlo. Para comentar el marcado declarativo, inclúyalo con los delimitadores `<%--` y `--%>`. Puede especificar estos delimitadores manualmente, o bien, como se muestra en la figura 2, puede seleccionar el texto que desea comentar y, a continuación, hacer clic en el icono marcar como comentario las líneas seleccionadas en la barra de herramientas. Del mismo modo, puede usar el icono marcar como comentario las líneas seleccionadas para comentar el código seleccionado en la clase de código subyacente.

[![comentar el marcado declarativo y el código fuente existentes en login. aspx](validating-user-credentials-against-the-membership-user-store-cs/_static/image5.png)](validating-user-credentials-against-the-membership-user-store-cs/_static/image4.png)

**Figura 2**: comentar el marcado declarativo y el código fuente existentes en `Login.aspx` ([haga clic para ver la imagen de tamaño completo](validating-user-credentials-against-the-membership-user-store-cs/_static/image6.png))

> [!NOTE]
> El icono marcar como comentario las líneas seleccionadas no está disponible cuando se ve el marcado declarativo en Visual Studio 2005. Si no usa Visual Studio 2008, deberá agregar manualmente los delimitadores `<%--` y `--%>`.

A continuación, arrastre un control login desde el cuadro de herramientas de a la página y establezca su propiedad `ID` en `myLogin`. En este punto, la pantalla debe tener un aspecto similar al de la figura 3. Tenga en cuenta que la interfaz predeterminada del control de inicio de sesión incluye controles de cuadro de texto para el nombre de usuario y la contraseña, una casilla recordarme próxima vez y un botón iniciar sesión. También hay `RequiredFieldValidator` controles para los dos cuadros de texto.

[![agregar un control de inicio de sesión a la página](validating-user-credentials-against-the-membership-user-store-cs/_static/image8.png)](validating-user-credentials-against-the-membership-user-store-cs/_static/image7.png)

**Figura 3**: agregar un control de inicio de sesión a la página ([haga clic para ver la imagen de tamaño completo](validating-user-credentials-against-the-membership-user-store-cs/_static/image9.png))

Y ya hemos terminado. Cuando se haga clic en el botón iniciar sesión del control de inicio de sesión, se producirá un postback y el control de inicio de sesión llamará al método `Membership.ValidateUser`, pasando el nombre de usuario y la contraseña especificados. Si las credenciales no son válidas, el control de inicio de sesión muestra un mensaje que indica que. Sin embargo, si las credenciales son válidas, el control de inicio de sesión crea el vale de autenticación de formularios y redirige al usuario a la página correspondiente.

El control de inicio de sesión utiliza cuatro factores para determinar la página adecuada a la que se redirigirá al usuario tras un inicio de sesión correcto:

- Si el control de inicio de sesión está en la página de inicio de sesión tal y como se define en `loginUrl` configuración de la autenticación de formularios. el valor predeterminado de esta configuración es `Login.aspx`
- La presencia de un parámetro QueryString `ReturnUrl`
- Valor de la [propiedad`DestinationUrl`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.destinationpageurl.aspx) del control de inicio de sesión.
- El valor de `defaultUrl` especificado en los valores de configuración de autenticación de formularios; el valor predeterminado de esta configuración es `Default.aspx`

En la figura 4 se describe el modo en que el control de inicio de sesión utiliza estos cuatro parámetros para llegar a su decisión de página adecuada.

[![agregar un control de inicio de sesión a la página](validating-user-credentials-against-the-membership-user-store-cs/_static/image11.png)](validating-user-credentials-against-the-membership-user-store-cs/_static/image10.png)

**Figura 4**: agregar un control de inicio de sesión a la página ([haga clic para ver la imagen de tamaño completo](validating-user-credentials-against-the-membership-user-store-cs/_static/image12.png))

Dedique un momento a probar el control de inicio de sesión visitando el sitio a través de un explorador e iniciar sesión como un usuario existente en el marco de pertenencia.

La interfaz representada del control de inicio de sesión es muy configurable. Hay una serie de propiedades que influyen en su apariencia; lo que es más, el control de inicio de sesión se puede convertir en una plantilla para un control preciso sobre el diseño de los elementos de la interfaz de usuario. En el resto de este paso se examina cómo personalizar la apariencia y el diseño.

### <a name="customizing-the-login-controls-appearance"></a>Personalizar la apariencia del control de inicio de sesión

La configuración predeterminada de las propiedades del control de inicio de sesión representa una interfaz de usuario con un título (inicio de sesión), controles de cuadro de texto y etiquetas para las entradas de nombre de usuario y contraseña, una casilla recordarme próxima vez y un botón iniciar sesión. Todos los aspectos de estos elementos se pueden configurar a través de las numerosas propiedades del control de inicio de sesión. Además, los elementos de la interfaz de usuario adicionales, como un vínculo a una página para crear una nueva cuenta de usuario, se pueden agregar estableciendo una o dos propiedades.

Vamos a pasar unos momentos para gussyr el aspecto del control de inicio de sesión. Dado que la página de `Login.aspx` ya tiene texto en la parte superior de la página que indica login, el título del control de inicio de sesión es superfluo. Por lo tanto, borre el valor de la [propiedad`TitleText`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.titletext.aspx) para quitar el título del control de inicio de sesión.

El nombre de usuario: y la contraseña: las etiquetas a la izquierda de los dos controles de cuadro de texto se pueden personalizar mediante las propiedades [`UserNameLabelText`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.usernamelabeltext.aspx) y [`PasswordLabelText`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.passwordlabeltext.aspx), respectivamente. Vamos a cambiar el nombre de usuario: Label por Read username:. Los estilos de etiqueta y de cuadro de texto se pueden configurar mediante las propiedades [`LabelStyle`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.labelstyle.aspx) y [`TextBoxStyle`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.textboxstyle.aspx), respectivamente.

La propiedad texto de la casilla recordarme próxima vez se puede establecer mediante el [`RememberMeText property`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.remembermetext.aspx)del control de inicio de sesión, y su estado de activación predeterminado es configurable a través de la [`RememberMeSet property`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.remembermeset.aspx) (cuyo valor predeterminado es false). Continúe y establezca la propiedad `RememberMeSet` en true para que la casilla recordarme próxima vez esté activada de forma predeterminada.

El control de inicio de sesión ofrece dos propiedades para ajustar el diseño de sus controles de interfaz de usuario. El [`TextLayout property`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.textlayout.aspx) indica si las etiquetas username: y password: aparecen a la izquierda de sus cuadros de texto correspondientes (el valor predeterminado), o por encima de ellas. El [`Orientation property`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.orientation.aspx) indica si las entradas de nombre de usuario y contraseña están ubicadas verticalmente (una encima de la otra) o horizontalmente. Voy a dejar estas dos propiedades establecidas en sus valores predeterminados, pero recomiendo que pruebe a establecer estas dos propiedades en sus valores no predeterminados para ver el efecto resultante.

> [!NOTE]
> En la siguiente sección, configuración del diseño del control de inicio de sesión, veremos el uso de plantillas para definir el diseño preciso de los elementos de la interfaz de usuario del control de diseño.

¿Desea ajustar la configuración de las propiedades del control de inicio de sesión estableciendo las propiedades [`CreateUserText`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.createusertext.aspx) y [`CreateUserUrl`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.createuserurl.aspx) en no registrado todavía? Cree una cuenta. y `~/Membership/CreatingUserAccounts.aspx`, respectivamente. Esto agrega un hipervínculo a la interfaz del control de inicio de sesión que apunta a la <a id="SKM6"> </a>página que hemos creado en el [tutorial anterior](creating-user-accounts-cs.md). Las propiedades [`HelpPageText`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.helppagetext.aspx) y [`HelpPageUrl`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.helppageurl.aspx) del control de inicio de sesión y las propiedades [`PasswordRecoveryText`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.passwordrecoverytext.aspx) y [`PasswordRecoveryUrl`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.passwordrecoveryurl.aspx) funcionan de la misma manera, lo que representa los vínculos a una página de ayuda y una página de recuperación de contraseña.

Después de realizar estos cambios en las propiedades, el marcado y la apariencia declarativos del control de inicio de sesión deben tener un aspecto similar al que se muestra en la figura 5.

[![los valores de las propiedades del control de inicio de sesión determinan su apariencia](validating-user-credentials-against-the-membership-user-store-cs/_static/image14.png)](validating-user-credentials-against-the-membership-user-store-cs/_static/image13.png)

**Figura 5**: los valores de las propiedades del control de inicio de sesión determinan su apariencia ([haga clic para ver la imagen de tamaño completo](validating-user-credentials-against-the-membership-user-store-cs/_static/image15.png))

### <a name="configuring-the-login-controls-layout"></a>Configurar el diseño del control de inicio de sesión

La interfaz de usuario predeterminada del control Web de inicio de sesión dispone de la interfaz en un `<table>`HTML. Pero ¿qué ocurre si necesitamos un mayor control sobre la salida representada? Es posible que deseemos reemplazar el `<table>` por una serie de etiquetas de `<div>`. ¿O si nuestra aplicación requiere credenciales adicionales para la autenticación? Muchos sitios web financieros, por ejemplo, requieren que los usuarios suministren no solo un nombre de usuario y una contraseña, sino también un número de identificación personal (PIN) u otra información de identificación. Sea cual sea el motivo, es posible convertir el control de inicio de sesión en una plantilla, desde donde podemos definir explícitamente el marcado declarativo de la interfaz.

Necesitamos hacer dos cosas para actualizar el control de inicio de sesión para recopilar credenciales adicionales:

1. Actualice la interfaz del control de inicio de sesión para incluir controles Web para recopilar las credenciales adicionales.
2. Invalide la lógica de autenticación interna del control de inicio de sesión para que un usuario solo se autentique si su nombre de usuario y contraseña son válidos y sus credenciales adicionales también son válidas.

Para llevar a cabo la primera tarea, es necesario convertir el control de inicio de sesión en una plantilla y agregar los controles Web necesarios. Como en la segunda tarea, la lógica de autenticación del control de inicio de sesión se puede reemplazar mediante la creación de un controlador de eventos para el [evento`Authenticate`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.authenticate.aspx)del control.

Vamos a actualizar el control de inicio de sesión para que solicite a los usuarios el nombre de usuario, la contraseña y la dirección de correo electrónico, y solo autentique al usuario si la dirección de correo electrónico proporcionada coincide con su dirección de correo electrónico en el archivo. En primer lugar, es necesario convertir la interfaz del control de inicio de sesión en una plantilla. En la etiqueta inteligente del control de inicio de sesión, elija la opción convertir en plantilla.

[![convertir el control de inicio de sesión en una plantilla](validating-user-credentials-against-the-membership-user-store-cs/_static/image17.png)](validating-user-credentials-against-the-membership-user-store-cs/_static/image16.png)

**Figura 6**: conversión del control de inicio de sesión en una plantilla ([haga clic para ver la imagen de tamaño completo](validating-user-credentials-against-the-membership-user-store-cs/_static/image18.png))

> [!NOTE]
> Para revertir el control de inicio de sesión a su versión anterior a la plantilla, haga clic en el vínculo de restablecimiento de la etiqueta inteligente del control.

Al convertir el control de inicio de sesión en una plantilla, se agrega una `LayoutTemplate` al marcado declarativo del control con elementos HTML y controles Web que definen la interfaz de usuario. Como se muestra en la figura 7, al convertir el control en una plantilla se quitan varias propiedades del ventana Propiedades, como `TitleText`, `CreateUserUrl`, etc., ya que estos valores de propiedad se omiten cuando se usa una plantilla.

[![hay menos propiedades disponibles cuando el control de inicio de sesión se convierte en una plantilla](validating-user-credentials-against-the-membership-user-store-cs/_static/image20.png)](validating-user-credentials-against-the-membership-user-store-cs/_static/image19.png)

**Figura 7**: hay menos propiedades disponibles cuando el control de inicio de sesión se convierte en una plantilla ([haga clic para ver la imagen a tamaño completo](validating-user-credentials-against-the-membership-user-store-cs/_static/image21.png))

El marcado HTML en el `LayoutTemplate` se puede modificar según sea necesario. Del mismo modo, no dude en agregar nuevos controles Web a la plantilla. Sin embargo, es importante que los controles Web principales del control de inicio de sesión permanezcan en la plantilla y mantengan los valores de `ID` asignados. En concreto, no quite ni cambie el nombre de los cuadros de texto `UserName` o `Password`, la casilla `RememberMe`, el botón `LoginButton`, la etiqueta `FailureText` o los controles `RequiredFieldValidator`.

Para recopilar la dirección de correo electrónico del visitante, es necesario agregar un cuadro de texto a la plantilla. Agregue el siguiente marcado declarativo entre la fila de la tabla (`<tr>`) que contiene el cuadro de texto `Password` y la fila de la tabla que contiene la casilla recordarme próxima vez:

[!code-aspx[Main](validating-user-credentials-against-the-membership-user-store-cs/samples/sample2.aspx)]

Después de agregar el cuadro de texto `Email`, visite la página a través de un explorador. Como se muestra en la figura 8, la interfaz de usuario del control de inicio de sesión ahora incluye un tercer cuadro de texto.

[![el control de inicio de sesión incluye ahora un cuadro de texto para la dirección de correo electrónico del usuario](validating-user-credentials-against-the-membership-user-store-cs/_static/image23.png)](validating-user-credentials-against-the-membership-user-store-cs/_static/image22.png)

**Figura 8**: el control de inicio de sesión ahora incluye un cuadro de texto para la dirección de correo electrónico del usuario ([haga clic para ver la imagen de tamaño completo](validating-user-credentials-against-the-membership-user-store-cs/_static/image24.png))

En este momento, el control de inicio de sesión sigue usando el método `Membership.ValidateUser` para validar las credenciales proporcionadas. En consecuencia, el valor especificado en el cuadro de texto `Email` no tiene en cuenta si el usuario puede iniciar sesión. En el paso 3, veremos cómo invalidar la lógica de autenticación del control de inicio de sesión para que las credenciales solo se consideren válidas si el nombre de usuario y la contraseña son válidos y la dirección de correo electrónico proporcionada coincide con la dirección de correo electrónico del archivo.

## <a name="step-3-modifying-the-login-controls-authentication-logic"></a>Paso 3: modificar la lógica de autenticación del control de inicio de sesión

Cuando un visitante suministra sus credenciales y hace clic en el botón iniciar sesión, se recorre un postback y el control de inicio de sesión progresa a través de su flujo de trabajo de autenticación. El flujo de trabajo se inicia generando el [evento`LoggingIn`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.loggingin.aspx). Cualquier controlador de eventos asociado a este evento puede cancelar la operación de inicio de sesión estableciendo la propiedad `e.Cancel` en `true`.

Si no se cancela la operación de inicio de sesión, el flujo de trabajo progresa generando el [evento`Authenticate`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.authenticate.aspx). Si hay un controlador de eventos para el evento `Authenticate`, es responsable de determinar si las credenciales proporcionadas son válidas o no. Si no se especifica ningún controlador de eventos, el control de inicio de sesión utiliza el método `Membership.ValidateUser` para determinar la validez de las credenciales.

Si las credenciales proporcionadas son válidas, se crea el vale de autenticación de formularios, se genera el [evento`LoggedIn`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.loggedin.aspx) y se redirige al usuario a la página correspondiente. Sin embargo, si las credenciales se consideran no válidas, se genera el [evento`LoginError`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.loginerror.aspx) y se muestra un mensaje que informa al usuario de que sus credenciales no son válidas. De forma predeterminada, en caso de error, el control de inicio de sesión simplemente establece su `FailureText` propiedad text del control etiqueta en un mensaje de error (el intento de inicio de sesión no se realizó correctamente). Vuelva a intentarlo). Sin embargo, si la [propiedad`FailureAction`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.failureaction.aspx) del control de inicio de sesión se establece en `RedirectToLoginPage`, el control de inicio de sesión emite un `Response.Redirect` a la página de inicio de sesión que anexa el parámetro QueryString `loginfailure=1` (que hace que el control de inicio de sesión muestre el mensaje de error).

En la ilustración 9 se ofrece un diagrama de flujo del flujo de trabajo de autenticación.

[![el flujo de trabajo de autenticación del control de inicio de sesión](validating-user-credentials-against-the-membership-user-store-cs/_static/image26.png)](validating-user-credentials-against-the-membership-user-store-cs/_static/image25.png)

**Figura 9**: flujo de trabajo de autenticación del control de inicio de sesión ([haga clic para ver la imagen de tamaño completo](validating-user-credentials-against-the-membership-user-store-cs/_static/image27.png))

> [!NOTE]
> Si se le pregunta si desea utilizar la opción de página `RedirectToLogin` del `FailureAction`, considere el siguiente escenario. En la actualidad, ahora nuestra `Site.master` página maestra tiene el texto, Hola, extraño mostrado en la columna izquierda cuando lo visita un usuario anónimo, pero Imagine que queríamos reemplazar ese texto por un control de inicio de sesión. Esto permitiría que un usuario anónimo iniciara sesión desde cualquier página del sitio, en lugar de requerir que visite directamente la página de inicio de sesión. Sin embargo, si un usuario no pudo iniciar sesión a través del control de inicio de sesión representado por la página maestra, puede que tenga sentido redirigirlos a la página de inicio de sesión (`Login.aspx`) porque es probable que esa página incluya instrucciones, vínculos y otra ayuda, como vínculos para crear una cuenta nueva o recuperar una contraseña perdida, que no se agregaron a la página maestra.

### <a name="creating-theauthenticateevent-handler"></a>Crear el controlador de eventos`Authenticate`

Para poder conectar la lógica de autenticación personalizada, es necesario crear un controlador de eventos para el evento `Authenticate` del control de inicio de sesión. Al crear un controlador de eventos para el evento `Authenticate`, se generará la siguiente definición de controlador de eventos:

[!code-csharp[Main](validating-user-credentials-against-the-membership-user-store-cs/samples/sample3.cs)]

Como puede ver, se pasa al controlador de eventos `Authenticate` un objeto de tipo [`AuthenticateEventArgs`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.authenticateeventargs.aspx) como su segundo parámetro de entrada. La clase `AuthenticateEventArgs` contiene una propiedad booleana denominada `Authenticated` que se utiliza para especificar si las credenciales proporcionadas son válidas. La tarea, a continuación, es escribir aquí el código que determina si las credenciales proporcionadas son válidas o no, y para establecer la propiedad `e.Authenticate` en consecuencia.

### <a name="determining-and-validating-the-supplied-credentials"></a>Determinación y validación de las credenciales proporcionadas

Use las propiedades [`UserName`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.username.aspx) y [`Password`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.password.aspx) del control de inicio de sesión para determinar las credenciales de nombre de usuario y contraseña especificadas por el usuario. Para determinar los valores especificados en los controles Web adicionales (como el `Email` cuadro de texto que hemos agregado en el paso anterior), use *`LoginControlID`* `.FindControl`(" *`controlID`* ") para obtener una referencia mediante programación al control Web en la plantilla cuya propiedad `ID` sea igual a *`controlID`* . Por ejemplo, para obtener una referencia al cuadro de texto `Email`, use el código siguiente:

`TextBox EmailTextBox = myLogin.FindControl("Email") as TextBox;`

Con el fin de validar las credenciales del usuario, es necesario hacer dos cosas:

1. Asegúrese de que el nombre de usuario y la contraseña proporcionados son válidos.
2. Asegúrese de que la dirección de correo electrónico especificada coincide con la dirección de correo electrónico del archivo para el usuario que intenta iniciar sesión.

Para realizar la primera comprobación, podemos usar simplemente el método `Membership.ValidateUser` como vimos en el paso 1. Para la segunda comprobación, es necesario determinar la dirección de correo electrónico del usuario para que podamos compararla con la dirección de correo electrónico introducida en el control de cuadro de texto. Para obtener información sobre un usuario determinado, utilice el [método`GetUser`](https://msdn.microsoft.com/library/system.web.security.membership.getuser.aspx)de la clase `Membership`.

El método `GetUser` tiene varias sobrecargas. Si se utiliza sin pasar ningún parámetro, devuelve información sobre el usuario que ha iniciado sesión actualmente. Para obtener información acerca de un usuario determinado, llame a `GetUser` pasando su nombre de usuario. En cualquier caso, `GetUser` devuelve un [objeto`MembershipUser`](https://msdn.microsoft.com/library/system.web.security.membershipuser.aspx), que tiene propiedades como `UserName`, `Email`, `IsApproved`, `IsOnline`, etc.

El código siguiente implementa estas dos comprobaciones. Si ambos pasen, `e.Authenticate` se establece en `true`; de lo contrario, se asigna `false`.

[!code-csharp[Main](validating-user-credentials-against-the-membership-user-store-cs/samples/sample4.cs)]

Con este código en su lugar, intente iniciar sesión como un usuario válido y escriba el nombre de usuario, la contraseña y la dirección de correo electrónico correctos. Inténtelo de nuevo, pero esta vez use de una dirección de correo electrónico incorrecta (consulte la figura 10). Por último, pruébelo una tercera vez con un nombre de usuario que no existe. En el primer caso, debe iniciar sesión correctamente en el sitio, pero en los dos últimos casos debería ver el mensaje de credenciales no válidos del control de inicio de sesión.

[![Tito no puede iniciar sesión al proporcionar una dirección de correo electrónico incorrecta](validating-user-credentials-against-the-membership-user-store-cs/_static/image29.png)](validating-user-credentials-against-the-membership-user-store-cs/_static/image28.png)

**Figura 10**: Tito no puede iniciar sesión al proporcionar una dirección de correo electrónico incorrecta ([haga clic para ver la imagen de tamaño completo](validating-user-credentials-against-the-membership-user-store-cs/_static/image30.png))

> [!NOTE]
> Como se describe en la sección Cómo controla el marco de pertenencia los intentos de inicio de sesión no válidos en el paso 1, cuando se llama al método `Membership.ValidateUser` y se pasan credenciales no válidas, realiza un seguimiento del intento de inicio de sesión no válido y bloquea al usuario si supera un umbral determinado de intentos no válidos dentro de un período de tiempo especificado. Puesto que nuestra lógica de autenticación personalizada llama al método `ValidateUser`, una contraseña incorrecta para un nombre de usuario válido incrementará el contador de intentos de inicio de sesión no válidos, pero este contador no se incrementará en el caso de que el nombre de usuario y la contraseña sean válidos, pero la dirección de correo electrónico no es correcta. Lo más probable es que este comportamiento sea adecuado, ya que es improbable que un pirata informático conozca el nombre de usuario y la contraseña, pero que tenga que usar técnicas de fuerza bruta para determinar la dirección de correo electrónico del usuario.

## <a name="step-4-improving-the-login-controls-invalid-credentials-message"></a>Paso 4: mejorar el mensaje de credenciales no válidas del control de inicio de sesión

Cuando un usuario intenta iniciar sesión con credenciales no válidas, el control de inicio de sesión muestra un mensaje que explica que el intento de inicio de sesión no se ha realizado correctamente. En concreto, el control muestra el mensaje especificado por su [`FailureText` propiedad](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.failuretext.aspx), que tiene un valor predeterminado de su intento de inicio de sesión no se ha realizado correctamente. Vuelva a intentarlo.

Recuerde que hay muchas razones por las que las credenciales de un usuario no son válidas:

- Puede que el nombre de usuario no exista
- El nombre de usuario existe, pero la contraseña no es válida
- El nombre de usuario y la contraseña son válidos, pero el usuario aún no se ha aprobado
- El nombre de usuario y la contraseña son válidos, pero el usuario está bloqueado (lo más probable es que haya superado el número de intentos de inicio de sesión no válidos en el período de tiempo especificado)

Y puede haber otras razones para usar la lógica de autenticación personalizada. Por ejemplo, con el código que escribimos en el paso 3, el nombre de usuario y la contraseña pueden ser válidos, pero la dirección de correo electrónico puede ser incorrecta.

Independientemente de la razón por la que las credenciales no sean válidas, el control de inicio de sesión muestra el mismo mensaje de error. Esta falta de comentarios puede ser confusa para un usuario cuya cuenta aún no se ha aprobado o que se ha bloqueado. Sin embargo, con un poco de trabajo, podemos hacer que el control de inicio de sesión muestre un mensaje más adecuado.

Cada vez que un usuario intenta iniciar sesión con credenciales no válidas, el control de inicio de sesión genera su evento `LoginError`. Continúe y cree un controlador de eventos para este evento y agregue el código siguiente:

[!code-csharp[Main](validating-user-credentials-against-the-membership-user-store-cs/samples/sample5.cs)]

El código anterior se inicia estableciendo la propiedad `FailureText` del control de inicio de sesión en el valor predeterminado (el intento de inicio de sesión no se ha realizado correctamente). Vuelva a intentarlo). A continuación, comprueba si el nombre de usuario proporcionado se asigna a una cuenta de usuario existente. Si es así, consulta las propiedades `IsLockedOut` y `IsApproved` del objeto de `MembershipUser` resultante para determinar si la cuenta se ha bloqueado o aún no se ha aprobado. En cualquier caso, la propiedad `FailureText` se actualiza a un valor correspondiente.

Para probar este código, intente iniciar sesión de forma intencionada como un usuario existente, pero use una contraseña incorrecta. Haga esto cinco veces en una fila en un período de 10 minutos y se bloqueará la cuenta. Como se muestra en la figura 11, los intentos de inicio de sesión subsiguientes siempre producirán un error (incluso con la contraseña correcta), pero ahora mostrarán el más descriptivo que se ha bloqueado su cuenta debido a demasiados intentos de inicio de sesión no válidos. Póngase en contacto con el administrador para que le desbloquee el mensaje de la cuenta.

[![Tito realizó demasiados intentos de inicio de sesión no válidos y se bloqueó](validating-user-credentials-against-the-membership-user-store-cs/_static/image32.png)](validating-user-credentials-against-the-membership-user-store-cs/_static/image31.png)

**Figura 11**: Tito realizó demasiados intentos de inicio de sesión no válidos y se bloqueó ([haga clic para ver la imagen de tamaño completo](validating-user-credentials-against-the-membership-user-store-cs/_static/image33.png))

## <a name="summary"></a>Resumen

Antes de este tutorial, la página de inicio de sesión validó las credenciales proporcionadas en una lista codificada de forma rígida de pares de nombre de usuario y contraseña. En este tutorial, se actualizó la página para validar las credenciales en el marco de pertenencia. En el paso 1 examinamos el uso del método `Membership.ValidateUser` mediante programación. En el paso 2 Hemos reemplazado la interfaz de usuario y el código creados manualmente con el control de inicio de sesión.

El control de inicio de sesión representa una interfaz de usuario de inicio de sesión estándar y valida automáticamente las credenciales del usuario con el marco de pertenencia. Además, en el caso de credenciales válidas, el control de inicio de sesión inicia la sesión del usuario mediante la autenticación de formularios. En Resumen, la experiencia del usuario de inicio de sesión totalmente funcional está disponible simplemente arrastrando el control de inicio de sesión a una página, sin necesidad de marcado o código declarativo adicional. Lo que es más, el control de inicio de sesión es muy personalizable, lo que permite un gran grado de control sobre la lógica de autenticación y la interfaz de usuario representada.

En este momento, los visitantes a nuestro sitio web pueden crear una nueva cuenta de usuario e iniciar sesión en el sitio, pero aún veremos cómo restringir el acceso a las páginas basadas en el usuario autenticado. Actualmente, cualquier usuario autenticado o anónimo puede ver cualquier página de nuestro sitio. Además de controlar el acceso a las páginas de un sitio por usuario, es posible que tengamos ciertas páginas cuya funcionalidad depende del usuario. En el siguiente tutorial se examina cómo limitar el acceso y la funcionalidad en página en función del usuario que ha iniciado sesión.

¡ Feliz programación!

### <a name="further-reading"></a>Información adicional

Para obtener más información sobre los temas descritos en este tutorial, consulte los siguientes recursos:

- [Mostrar mensajes personalizados a usuarios bloqueados y no aprobados](http://aspnet.4guysfromrolla.com/articles/050306-1.aspx)
- [Examen de la pertenencia, los roles y el perfil de ASP.NET 2.0](http://aspnet.4guysfromrolla.com/articles/120705-1.aspx)
- [Cómo: crear una página de inicio de sesión de ASP.NET](https://msdn.microsoft.com/library/ms178331.aspx)
- [Documentación técnica del control de inicio de sesión](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.aspx)
- [Usar los controles de inicio de sesión](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/security/login.aspx)

### <a name="about-the-author"></a>Acerca del autor

Scott Mitchell, autor de varios libros de ASP/ASP. NET y fundador de 4GuysFromRolla.com, ha estado trabajando con las tecnologías Web de Microsoft desde 1998. Scott funciona como consultor, profesor y redactor independiente. Su último libro se *[enseña a ASP.NET 2,0 en 24 horas](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)* . Se puede acceder a Scott en [mitchell@4guysfromrolla.com](mailto:mitchell@4guysfromrolla.com) o a través de su blog en [http://ScottOnWriting.NET](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Agradecimiento especial a

Muchos revisores útiles revisaron esta serie de tutoriales. Los revisores responsables de este tutorial eran Teresa Murphy y Michael Olivero. ¿Está interesado en revisar los próximos artículos de MSDN? En caso afirmativo, suéltelo en [mitchell@4GuysFromRolla.com](mailto:mitchell@4guysfromrolla.com).

> [!div class="step-by-step"]
> [Anterior](creating-user-accounts-cs.md)
> [Siguiente](user-based-authorization-cs.md)
