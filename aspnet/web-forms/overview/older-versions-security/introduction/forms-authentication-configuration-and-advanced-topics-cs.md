---
uid: web-forms/overview/older-versions-security/introduction/forms-authentication-configuration-and-advanced-topics-cs
title: Configuración de la autenticación de formularios yC#temas avanzados () | Microsoft Docs
author: rick-anderson
description: En este tutorial, examinaremos las distintas configuraciones de autenticación de formularios y veremos cómo modificarlas mediante el elemento Forms. Esto supondrá un...
ms.author: riande
ms.date: 01/14/2008
ms.assetid: b9c29865-a34e-48bb-92c0-c443a72cb860
msc.legacyurl: /web-forms/overview/older-versions-security/introduction/forms-authentication-configuration-and-advanced-topics-cs
msc.type: authoredcontent
ms.openlocfilehash: b296f31da1c73df97175d94402b4d618df425d8d
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/28/2019
ms.locfileid: "74579292"
---
# <a name="forms-authentication-configuration-and-advanced-topics-c"></a>Configuración de la autenticación de formularios y temas avanzados (C#)

por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descargar código](https://download.microsoft.com/download/2/F/7/2F705A34-F9DE-4112-BBDE-60098089645E/ASPNET_Security_Tutorial_03_CS.zip) o [Descargar PDF](https://download.microsoft.com/download/2/F/7/2F705A34-F9DE-4112-BBDE-60098089645E/aspnet_tutorial03_AuthAdvanced_cs.pdf)

> En este tutorial, examinaremos las distintas configuraciones de autenticación de formularios y veremos cómo modificarlas mediante el elemento Forms. Esto implicará una visión detallada de la personalización del valor de tiempo de espera del vale de autenticación de formularios, mediante una página de inicio de sesión con una dirección URL personalizada (como inicio de sesión. aspx en lugar de login. aspx) y vales de autenticación de formularios sin cookies.

## <a name="introduction"></a>Introducción

En el [tutorial anterior](an-overview-of-forms-authentication-cs.md) , hemos examinado los pasos necesarios para implementar la autenticación de formularios en una aplicación de ASP.net, desde la especificación de la configuración de Web. config hasta la creación de una página de inicio de sesión para mostrar el contenido diferente para los usuarios autenticados y anónimos. Recuerde que hemos configurado el sitio web para que use la autenticación de formularios estableciendo el atributo de modo del elemento &lt;Authentication&gt; en Forms. El elemento &lt;Authentication&gt; puede incluir opcionalmente un &lt;Forms&gt; elemento secundario, a través del cual se puede especificar una serie de valores de autenticación de formularios.

En este tutorial, examinaremos las distintas configuraciones de autenticación de formularios y veremos cómo modificarlas mediante el elemento &lt;Forms&gt;. Esto implicará una visión detallada de la personalización del valor de tiempo de espera del vale de autenticación de formularios, mediante una página de inicio de sesión con una dirección URL personalizada (como inicio de sesión. aspx en lugar de login. aspx) y vales de autenticación de formularios sin cookies. También examinaremos la composición del vale de autenticación de formularios más detenidamente y veremos las precauciones que ASP.NET toma para asegurarse de que los datos del vale están seguros para la inspección y manipulación. Por último, veremos cómo almacenar datos de usuario adicionales en el vale de autenticación de formularios y cómo modelar estos datos a través de un objeto de entidad de seguridad personalizado.

## <a name="step-1-examining-the-ltformsgt-configuration-settings"></a>Paso 1: examinar los valores de configuración de&gt; de &lt;Forms

El sistema de autenticación de formularios de ASP.NET ofrece una serie de opciones de configuración que se pueden personalizar aplicación por aplicación. Esto incluye valores como: la duración del vale de autenticación de formularios; el tipo de protección que se aplica al vale; en qué condiciones se utilizan vales de autenticación sin cookies; la ruta de acceso a la página de inicio de sesión; y otra información. Para modificar los valores predeterminados, agregue un [&lt;forms&gt; elemento](https://msdn.microsoft.com/library/1d3t3c61.aspx) como elemento secundario del [elemento&lt;Authentication&gt;](https://msdn.microsoft.com/library/532aee0e.aspx), especificando los valores de propiedad que desea personalizar como atributos XML como:

[!code-xml[Main](forms-authentication-configuration-and-advanced-topics-cs/samples/sample1.xml)]

En la tabla 1 se resumen las propiedades que se pueden personalizar mediante el elemento &lt;Forms&gt;. Como Web. config es un archivo XML, los nombres de atributo de la columna izquierda distinguen mayúsculas de minúsculas.

| <strong>Attribute</strong> |                                                                                                                                                                                                                                     <strong>Descripción</strong>                                                                                                                                                                                                                                      |
|----------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|         sin cookies         |                                                                                                                Este atributo especifica en qué condiciones se almacena el vale de autenticación en una cookie en lugar de incrustarse en la dirección URL. Los valores permitidos son: UseCookies; UseUri; Detección automática y UseDeviceProfile (valor predeterminado). En el paso 2 se examina esta configuración con más detalle.                                                                                                                |
|         defaultUrl         |                                                                                                                                                         Indica la dirección URL a la que se redirige a los usuarios después de iniciar sesión desde la página de inicio de sesión si no hay ningún valor de RedirectUrl especificado en la cadena de entrada. El valor predeterminado es default. aspx.                                                                                                                                                         |
|           dominio           | Al utilizar vales de autenticación basada en cookies, esta configuración especifica el valor de dominio de la cookie. El valor predeterminado es una cadena vacía, que hace que el explorador use el dominio desde el que se emitió (por ejemplo, www.yourdomain.com). En este caso, la cookie <strong>no</strong> se enviará al realizar solicitudes a subdominios, como admin.yourdomain.com. Si desea que la cookie se pase a todos los subdominios, debe personalizar el atributo de dominio estableciéndolo en yourdomain.com. |
|  enableCrossAppRedirects   |                                                                                                                                                                   Valor booleano que indica si se recuerdan usuarios autenticados cuando se redirigen a direcciones URL de otras aplicaciones web en el mismo servidor. El valor predeterminado es false.                                                                                                                                                                   |
|          loginUrl          |                                                                                                                                                                                                                      Dirección URL de la página de inicio de sesión. El valor predeterminado es login. aspx.                                                                                                                                                                                                                      |
|            {2&gt;nombre&lt;2}            |                                                                                                                                                                                                   Al utilizar vales de autenticación basada en cookies, el nombre de la cookie. El valor predeterminado es. ASPXAUTH.                                                                                                                                                                                                   |
|            ruta de acceso            |                                                                             Al utilizar vales de autenticación basada en cookies, esta configuración especifica el atributo de ruta de acceso de la cookie. El atributo path permite a un programador limitar el ámbito de una cookie a una jerarquía de directorios determinada. El valor predeterminado es/, que informa al explorador de que envíe la cookie de vale de autenticación a cualquier solicitud realizada al dominio.                                                                              |
|         protección         |                                                                                                                                            Indica las técnicas que se usan para proteger el vale de autenticación de formularios. Los valores permitidos son: ALL (el valor predeterminado); Cifra Ninguna y validación. Esta configuración se describe en detalle en el paso 3.                                                                                                                                            |
|         requireSSL         |                                                                                                                                                                                Valor booleano que indica si se requiere una conexión SSL para transmitir la cookie de autenticación. El valor predeterminado es false.                                                                                                                                                                                |
|     slidingExpiration      |                                                                                                 Valor booleano que indica si el tiempo de espera de la cookie de autenticación se restablece cada vez que el usuario visita el sitio durante una sola sesión. El valor predeterminado es true. La Directiva de tiempo de espera del vale de autenticación se describe con más detalle en la sección especificación del valor de tiempo de espera del vale.                                                                                                 |
|          tiempo de espera           |                                                                                                                               Especifica el tiempo, en minutos, después del cual expira la cookie del vale de autenticación. El valor predeterminado es 30. La Directiva de tiempo de espera del vale de autenticación se describe con más detalle en la sección especificación del valor de tiempo de espera del vale.                                                                                                                               |

**Tabla 1**: Resumen de los atributos del elemento&gt; de los formularios &lt;

En ASP.NET 2,0 y versiones posteriores, los valores predeterminados de autenticación de formularios están codificados de forma rígida en la clase FormsAuthenticationConfiguration en el .NET Framework. Cualquier modificación debe aplicarse aplicación por aplicación en el archivo Web. config. Esto difiere de ASP.NET 1. x, donde los valores de autenticación de formularios predeterminados se almacenan en el archivo Machine. config (y, por tanto, se pueden modificar mediante la edición de Machine. config). Aunque en el tema de ASP.NET 1. x, merece la pena mencionar que una serie de valores de configuración del sistema de autenticación de formularios tienen distintos valores predeterminados en ASP.NET 2,0 y más que en ASP.NET 1. x. Si va a migrar la aplicación desde un entorno de ASP.NET 1. x, es importante tener en cuenta estas diferencias. Consulte [la documentación técnica del elemento de &lt;&gt;](https://msdn.microsoft.com/library/1d3t3c61.aspx) para obtener una lista de las diferencias.

> [!NOTE]
> Varias opciones de autenticación de formularios, como el tiempo de espera, el dominio y la ruta de acceso, especifican los detalles de la cookie de vale de autenticación de formularios resultante. Para obtener más información sobre las cookies, cómo funcionan y sus distintas propiedades, lea [este tutorial de cookies](http://www.quirksmode.org/js/cookies.html).

### <a name="specifying-the-tickets-timeout-value"></a>Especificar el valor de tiempo de espera del vale

El vale de autenticación de formularios es un token que representa una identidad. Con vales de autenticación basada en cookies, este token se mantiene en forma de una cookie y se envía al servidor Web en cada solicitud. La posesión del token, en esencia, declara, soy *nombreDeUsuario*, ya he iniciado sesión y se usa para que se pueda recordar la identidad de un usuario a través de visitas a la página.

El vale de autenticación de formularios no solo incluye la identidad del usuario, sino que también contiene información para ayudar a garantizar la integridad y la seguridad del token. Después de todo, no queremos que un usuario de fraudulento pueda crear un token falsificado o modificar un token de legit de forma subsiguiente.

Uno de estos bits de información incluido en el vale es una *expiración*, que es la fecha y la hora en que el vale ya no es válido. Cada vez que el FormsAuthenticationModule inspecciona un vale de autenticación, garantiza que la expiración del vale todavía no se ha superado. Si es así, no se tiene en cuenta el vale e identifica al usuario como anónimo. Esta medida de seguridad ayuda a protegerse frente a ataques de reproducción. Sin una expiración, si un pirata informático pudo obtener el vale de autenticación válido de un usuario, quizás al obtener acceso físico a su equipo y a la raíz a través de sus cookies, podría enviar una solicitud al servidor con este vale de autenticación robado y entrada de ganancia. Aunque la expiración no impide este escenario, limita la ventana durante la cual se puede realizar este tipo de ataque.

> [!NOTE]
> En el paso 3 se detallan técnicas adicionales utilizadas por el sistema de autenticación de formularios para proteger el vale de autenticación.

Al crear el vale de autenticación, el sistema de autenticación de formularios determina su expiración mediante la consulta de la configuración de tiempo de espera. Como se indica en la tabla 1, el valor de tiempo de espera predeterminado es 30 minutos, lo que significa que, cuando se crea el vale de autenticación de formularios, su expiración se establece en una fecha y hora 30 minutos en el futuro.

La expiración define una hora absoluta en el futuro cuando expira el vale de autenticación de formularios. Pero normalmente los desarrolladores desean implementar una expiración variable, una que se restablece cada vez que el usuario vuelve a visitar el sitio. Este comportamiento está determinado por la configuración de slidingExpiration. Si se establece en true (valor predeterminado), cada vez que el FormsAuthenticationModule autentique a un usuario, actualiza la expiración del vale. Si se establece en false, la expiración no se actualiza en cada solicitud, lo que hace que el vale expire exactamente el tiempo de espera hasta que se creó el vale por primera vez.

> [!NOTE]
> La expiración almacenada en el vale de autenticación es un valor de fecha y hora absoluto, como el 2 de agosto de 2008 a las 11:34 A.M. Además, la fecha y la hora son relativas a la hora local del servidor Web. Esta decisión de diseño puede tener algunos efectos secundarios interesantes sobre el horario de verano (DST), que es cuando los relojes del Estados Unidos se mueven antes de una hora (suponiendo que el servidor web está hospedado en una configuración regional donde se observa el horario de verano). Tenga en cuenta lo que sucedería para un sitio web de ASP.NET con una expiración de 30 minutos cerca del momento en que se inicia el horario de verano (que es a 2:00 AM). Imagine que un visitante inicia sesión en el sitio el 11 de marzo de 2008 a las 1:55 a. m. Esto generaría un vale de autenticación de formularios que expira el 11 de marzo de 2008 a las 2:25 AM (30 minutos en el futuro). Sin embargo, una vez que se retiran las 2:00, el reloj salta a 3:00 AM debido al horario de verano. Cuando el usuario carga una nueva página seis minutos después de iniciar sesión (a las 3:01 AM), el FormsAuthenticationModule anota que el vale ha expirado y redirige al usuario a la página de inicio de sesión. Para obtener una explicación más detallada sobre este y otros singularidades de tiempo de espera de vales de autenticación, así como soluciones alternativas, seleccione una copia de la *seguridad, la pertenencia y la administración de roles de Stefan Schackow Professional ASP.NET 2,0* (ISBN: 978-0-7645-9698-8).

En la figura 1 se muestra el flujo de trabajo cuando slidingExpiration está establecido en false y el tiempo de espera se establece en 30. Tenga en cuenta que el vale de autenticación generado en login contiene la fecha de expiración, y este valor no se actualiza en las solicitudes posteriores. Si FormsAuthenticationModule detecta que el vale ha expirado, lo descarta y trata la solicitud como anónima.

[![una representación gráfica de la expiración del vale de autenticación de formularios cuando slidingExpiration es false](forms-authentication-configuration-and-advanced-topics-cs/_static/image2.png)](forms-authentication-configuration-and-advanced-topics-cs/_static/image1.png)

**Figura 01**: representación gráfica de la expiración del vale de autenticación de formularios cuando slidingExpiration es false ([haga clic para ver la imagen de tamaño completo](forms-authentication-configuration-and-advanced-topics-cs/_static/image3.png))

En la ilustración 2 se muestra el flujo de trabajo cuando slidingExpiration está establecido en true y el tiempo de espera se establece en 30. Cuando se recibe una solicitud autenticada (con un vale que no ha expirado), la expiración se actualiza al número de minutos de tiempo de espera en el futuro.

[![una representación gráfica del vale de autenticación de formularios cuando slidingExpiration es true](forms-authentication-configuration-and-advanced-topics-cs/_static/image5.png)](forms-authentication-configuration-and-advanced-topics-cs/_static/image4.png)

**Figura 02**: representación gráfica del vale de autenticación de formularios cuando slidingExpiration es true ([haga clic para ver la imagen de tamaño completo](forms-authentication-configuration-and-advanced-topics-cs/_static/image6.png))

Cuando se utilizan vales de autenticación basada en cookies (valor predeterminado), esta explicación se vuelve un poco más confusa, ya que las cookies también pueden tener sus propios expiradores. La expiración de una cookie (o su falta) indica al explorador Cuándo debe destruirse la cookie. Si la cookie no tiene expiración, se destruye cuando el explorador se cierra. Sin embargo, si hay una expiración, la cookie permanece almacenada en el equipo del usuario hasta que se supera la fecha y hora especificadas en la expiración. Cuando el explorador destruye una cookie, ya no se envía al servidor Web. Por lo tanto, la destrucción de una cookie es análoga al inicio de sesión del usuario en el sitio.

> [!NOTE]
> Por supuesto, un usuario puede quitar de forma proactiva las cookies almacenadas en su equipo. En Internet Explorer 7, puede ir a herramientas, opciones y hacer clic en el botón eliminar en la sección historial de exploración. Desde allí, haga clic en el botón Eliminar cookies.

El sistema de autenticación de formularios crea cookies basadas en sesión o expiradas según el valor que se pasa al parámetro *persistCookie* . Recuerde que los métodos GetAuthCookie, SetAuthCookie y RedirectFromLoginPage de la clase FormsAuthentication toman dos parámetros de entrada: *username* y *persistCookie*. La página de inicio de sesión que creamos en el tutorial anterior incluía una casilla recordarme, que determina si se ha creado una cookie persistente. Las cookies persistentes están basadas en la expiración; las cookies no persistentes están basadas en sesiones.

Los conceptos de tiempo de espera y slidingExpiration ya descritos se aplican a las cookies basadas en sesión y en la expiración. Solo hay una diferencia menor en la ejecución: cuando se usan cookies basadas en expiración con slidingTimeout establecido en true, la expiración de la cookie solo se actualiza cuando ha transcurrido más de la mitad del tiempo especificado.

Vamos a actualizar las directivas de tiempo de espera de los vales de autenticación de su sitio web para que los vales expiren después de una hora (60 minutos), usando una expiración variable. Para aplicar este cambio, actualice el archivo Web. config, agregando un &lt;&gt; elemento en el elemento &lt;Authentication&gt; con el siguiente marcado:

[!code-xml[Main](forms-authentication-configuration-and-advanced-topics-cs/samples/sample2.xml)]

### <a name="using-an-login-page-url-other-than-loginaspx"></a>Usar una dirección URL de página de inicio de sesión distinta de login. aspx

Dado que FormsAuthenticationModule redirige automáticamente a los usuarios no autorizados a la página de inicio de sesión, debe conocer la dirección URL de la página de inicio de sesión. Esta dirección URL se especifica mediante el atributo loginUrl en el elemento &lt;Forms&gt; y el valor predeterminado es login. aspx. Si va a migrar a través de un sitio web existente, es posible que ya tenga una página de inicio de sesión con una dirección URL diferente, una que ya se haya marcado e indexado por motores de búsqueda. En lugar de cambiar el nombre de la página de inicio de sesión existente a login. aspx y romper los vínculos y los marcadores de los usuarios, puede modificar el atributo loginUrl para que apunte a la página de inicio de sesión.

Por ejemplo, si la página de inicio de sesión se denominaba signon. aspx y se encontraba en el directorio usuarios, podría apuntar a la opción de configuración loginUrl a ~/Users/SignIn.aspx como se indica a continuación:

[!code-xml[Main](forms-authentication-configuration-and-advanced-topics-cs/samples/sample3.xml)]

Puesto que nuestra aplicación actual ya tiene una página de inicio de sesión denominada login. aspx, no es necesario especificar un valor personalizado en el elemento &lt;Forms&gt;.

## <a name="step-2-using-cookieless-forms-authentication-tickets"></a>Paso 2: uso de vales de autenticación de formularios sin cookies

De forma predeterminada, el sistema de autenticación de formularios determina si se deben almacenar sus vales de autenticación en la colección de cookies o insertarlos en la dirección URL según el agente de usuario que visita el sitio. Todos los exploradores de escritorio estándar, como Internet Explorer, Firefox, opera y Safari, admiten cookies, pero no todos los dispositivos móviles.

La Directiva de cookies que usa el sistema de autenticación de formularios depende de la configuración sin cookies del elemento &lt;Forms&gt;, al que se le puede asignar uno de cuatro valores:

- UseCookies: especifica que siempre se usarán vales de autenticación basados en cookies.
- UseUri: indica que nunca se usarán vales de autenticación basados en cookies.
- Detección automática: Si el perfil de dispositivo no admite cookies, no se utilizan vales de autenticación basada en cookies. Si el perfil de dispositivo admite cookies, se usa un mecanismo de sondeo para determinar si las cookies están habilitadas.
- UseDeviceProfile: el valor predeterminado; usa vales de autenticación basada en cookies solo si el perfil de dispositivo admite cookies. No se usa ningún mecanismo de sondeo.

La configuración de detección automática y UseDeviceProfile se basa en un *Perfil de dispositivo* para determinar si se deben usar vales de autenticación basados en cookies o sin cookies. ASP.NET mantiene una base de datos de varios dispositivos y sus capacidades, como, por ejemplo, si admiten cookies, qué versión de JavaScript admiten, etc. Cada vez que un dispositivo solicita una página web de un servidor Web que envía a lo largo de un encabezado HTTP *User-Agent* que identifica el tipo de dispositivo. ASP.NET coincide automáticamente con la cadena de agente de usuario proporcionada con el perfil correspondiente especificado en su base de datos.

> [!NOTE]
> Esta base de datos de funcionalidades del dispositivo se almacena en varios archivos XML que se ajustan al [esquema del archivo de definición del explorador](https://msdn.microsoft.com/library/ms228122.aspx). Los archivos de Perfil de dispositivo predeterminados se encuentran en%WINDIR%\Microsoft.Net\Framework\v2.0.50727\CONFIG\Browsers. También puede Agregar archivos personalizados a la carpeta de aplicaciones\_exploradores de la aplicación. Para obtener más información, consulte [Cómo: detectar tipos de explorador en ASP.NET Web pages](https://msdn.microsoft.com/library/3yekbd5b.aspx).

Dado que el valor predeterminado es UseDeviceProfile, se usarán vales de autenticación de formularios sin cookies cuando el sitio sea visitado por un dispositivo cuyo perfil notifique que no admite cookies.

### <a name="encoding-the-authentication-ticket-in-the-url"></a>Codificar el vale de autenticación en la dirección URL

Las cookies son un medio natural para incluir información desde el explorador en cada solicitud a un sitio Web determinado, por lo que la configuración de autenticación de formularios predeterminada usa cookies si el dispositivo visitante las admite. Si no se admiten cookies, debe emplearse un medio alternativo para pasar el vale de autenticación desde el cliente al servidor. Una solución común que se usa en entornos sin cookies es codificar los datos de cookies en la dirección URL.

La mejor manera de ver cómo se puede incrustar esta información en la dirección URL es obligar al sitio a utilizar vales de autenticación sin cookies. Esto puede realizarse estableciendo la opción de configuración sin cookies en UseUri:

[!code-xml[Main](forms-authentication-configuration-and-advanced-topics-cs/samples/sample4.xml)]

Una vez realizado este cambio, visite el sitio a través de un explorador. Al visitar como un usuario anónimo, las direcciones URL se verán exactamente igual que antes. Por ejemplo, al visitar la página default. aspx, la barra de direcciones del explorador muestra la siguiente dirección URL:

`http://localhost:2448/ASPNET\_Security\_Tutorial\_03\_CS/default.aspx`

Sin embargo, al iniciar sesión, el vale de autenticación de formularios se incrusta en la dirección URL. Por ejemplo, después de visitar la página de inicio de sesión e iniciar sesión como Sam, se vuelve a la página default. aspx, pero la dirección URL es:

`http://localhost:2448/ASPNET\_Security\_Tutorial\_03\_CS/(F(jaIOIDTJxIr12xYS-VVgkqKCVAuIoW30Bu0diWi6flQC-FyMaLXJfow\_Vd9GZkB2Cv-rfezq0gKadKX0YPZCkA2))/default.aspx`

El vale de autenticación de formularios se ha incrustado en la dirección URL. La cadena (F (jaIOIDTJxIr12xYS-VVgkqKCVAuIoW30Bu0diWi6flQC-FyMaLXJfow\_Vd9GZkB2Cv-rfezq0gKadKX0YPZCkA2) representa la información del vale de autenticación codificada en hexadecimal y es el mismo dato que normalmente se almacena en una cookie.

Para que los vales de autenticación sin cookies funcionen, el sistema debe codificar todas las direcciones URL de la página para incluir los datos del vale de autenticación; de lo contrario, el vale de autenticación se perderá cuando el usuario haga clic en un vínculo. Afortunadamente, esta lógica de inserción se realiza automáticamente. Para demostrar esta funcionalidad, abra la página default. aspx y agregue un control de hipervínculo, estableciendo sus propiedades Text y NavigateUrl en test Link y SomePage. aspx, respectivamente. No importa que realmente no haya una página en nuestro proyecto llamada SomePage. aspx.

Guarde los cambios en default. aspx y, a continuación, visítelo a través de un explorador. Inicie sesión en el sitio para que el vale de autenticación de formularios se incruste en la dirección URL. A continuación, en default. aspx, haga clic en el vínculo probar vínculo. ¿Qué ocurre? Si no existe ninguna página denominada SomePage. aspx, se produjo un error 404, pero esto no es importante aquí. En su lugar, céntrese en la barra de direcciones del explorador. Tenga en cuenta que incluye el vale de autenticación de formularios en la dirección URL.

`http://localhost:2448/ASPNET\_Security\_Tutorial\_03\_CS/(F(jaIOIDTJxIr12xYS-VVgkqKCVAuIoW30Bu0diWi6flQC-FyMaLXJfow\_Vd9GZkB2Cv-rfezq0gKadKX0YPZCkA2))/SomePage.aspx`

La dirección URL SomePage. aspx en el vínculo se convirtió automáticamente en una dirección URL que incluía el vale de autenticación (no tuvimos que escribir un código). El vale de autenticación de formularios se incrustará automáticamente en la dirección URL de los hipervínculos que no empiecen por `http://` o `/`. No importa si el hipervínculo aparece en una llamada a Response. Redirect, en un control de hipervínculo o en un elemento HTML de delimitador (es decir, `<a href="...">...</a>`). Siempre que la dirección URL no sea similar a `http://www.someserver.com/SomePage.aspx` o `/SomePage.aspx`, se insertará el vale de autenticación de formularios.

> [!NOTE]
> Los vales de autenticación de formularios sin cookies se ajustan a las mismas directivas de tiempo de espera que los vales de autenticación basados en cookies. No obstante, los vales de autenticación sin cookies son más propensos a ataques de reproducción, ya que el vale de autenticación se incrusta directamente en la dirección URL. Imagine un usuario que visita un sitio web, inicia sesión y pega la dirección URL en un correo electrónico a un colega. Si el colega hace clic en ese vínculo antes de que se alcance la expiración, se iniciará sesión como el usuario que envió el correo electrónico.

## <a name="step-3-securing-the-authentication-ticket"></a>Paso 3: proteger el vale de autenticación

El vale de autenticación de formularios se transmite a través de la conexión, ya sea en una cookie o insertada directamente en la dirección URL. Además de la información de identidad, el vale de autenticación también puede incluir datos de usuario (como veremos en el paso 4). Por lo tanto, es importante que los datos del vale se cifren de los ojos no contratados y (incluso más importante) que el sistema de autenticación de formularios pueda garantizar que el vale no se ha manipulado.

Para garantizar la privacidad de los datos del vale, el sistema de autenticación de formularios puede cifrar los datos del vale. Si no se cifran los datos del vale, se envía información potencialmente confidencial a través de la conexión en texto sin formato.

Para garantizar la autenticidad de un vale, el sistema de autenticación de formularios debe *validar* el vale. La validación es el acto de asegurarse de que un determinado fragmento de datos no se ha modificado y se realiza a través de un *[código de autenticación de mensajes (Mac)](http://en.wikipedia.org/wiki/Message_authentication_code)* . En pocas palabras, el equipo MAC es un pequeño fragmento de información que identifica los datos que deben validarse (en este caso, el vale). Si se modifican los datos representados por el equipo MAC, el equipo MAC y los datos no coincidirán. Además, es computacionalmente difícil que un pirata informático modifique los datos y genere su propio equipo MAC para que se corresponda con los datos modificados.

Al crear (o modificar) un vale, el sistema de autenticación de formularios crea un equipo MAC y lo adjunta a los datos del vale. Cuando llega una solicitud posterior, el sistema de autenticación de formularios compara los datos MAC y ticket para validar la autenticidad de los datos de los vales. La figura 3 ilustra este flujo de trabajo de forma gráfica.

[![la autenticidad del vale se garantiza a través de un equipo MAC](forms-authentication-configuration-and-advanced-topics-cs/_static/image8.png)](forms-authentication-configuration-and-advanced-topics-cs/_static/image7.png)

**Figura 03**: la autenticidad del vale se garantiza a través de un equipo Mac ([haga clic para ver la imagen de tamaño completo](forms-authentication-configuration-and-advanced-topics-cs/_static/image9.png))

Las medidas de seguridad que se aplican al vale de autenticación dependen de la configuración de protección del elemento &lt;Forms&gt;. La configuración de protección se puede asignar a uno de los tres valores siguientes:

- All: el vale se cifra y se firma digitalmente (valor predeterminado).
- Se aplica el cifrado solo de cifrado: no se genera ningún equipo MAC.
- Ninguno: el vale no está cifrado ni firmado digitalmente.
- Validación: se genera un equipo MAC, pero los datos de los vales se envían a través de la conexión en texto sin formato.

Microsoft recomienda encarecidamente usar la configuración todos.

### <a name="setting-the-validation-and-decryption-keys"></a>Establecimiento de las claves de validación y descifrado

Los algoritmos de cifrado y hash utilizados por el sistema de autenticación de formularios para cifrar y validar el vale de autenticación se pueden personalizar a través del [elemento&lt;machineKey&gt;](https://msdn.microsoft.com/library/w8h3skw9.aspx) en Web. config. En la tabla 2 se describen los atributos del elemento &lt;machineKey&gt; y sus valores posibles.

| **Attribute** | **Descripción** |
| --- | --- |
| descifrado | Indica el algoritmo utilizado para el cifrado. Este atributo puede tener uno de los cuatro valores siguientes:-auto-el valor predeterminado; determina el algoritmo en función de la longitud del atributo decryptionKey. -AES: usa el algoritmo de [estándar de cifrado avanzado (AES)](http://en.wikipedia.org/wiki/Advanced_Encryption_Standard) . -DES: usa el [estándar de cifrado de datos (des)](http://en.wikipedia.org/wiki/Data_Encryption_Standard) . este algoritmo se considera poco seguro y no debe usarse. -3DES: usa el algoritmo des [triple](http://en.wikipedia.org/wiki/Triple_DES) , que funciona aplicando el algoritmo des tres veces. |
| decryptionKey | Clave secreta utilizada por el algoritmo de cifrado. Este valor debe ser una cadena hexadecimal de la longitud adecuada (basada en el valor en el descifrado), generar automáticamente o cualquier valor anexado con, IsolateApps. Agregar IsolateApps indica a ASP.NET que use un valor único para cada aplicación. El valor predeterminado es AutoGenerate, IsolateApps. |
| validación | Indica el algoritmo utilizado para la validación. Este atributo puede tener uno de los cuatro valores siguientes:-AES-usa el algoritmo Estándar de cifrado avanzado (AES). -MD5: usa el algoritmo [Message-Digest 5 (MD5)](http://en.wikipedia.org/wiki/MD5) . -SHA1: usa el algoritmo [SHA1](http://en.wikipedia.org/wiki/Sha1) (el valor predeterminado). -3DES: usa el algoritmo DES triple. |
| validationKey | Clave secreta utilizada por el algoritmo de validación. Este valor debe ser una cadena hexadecimal de la longitud adecuada (según el valor de validación), autogenerar o cualquier valor anexado con, IsolateApps. Agregar IsolateApps indica a ASP.NET que use un valor único para cada aplicación. El valor predeterminado es AutoGenerate, IsolateApps. |

**Tabla 2**: atributos del elemento &lt;machineKey&gt;

Un análisis exhaustivo de estas opciones de cifrado y validación, así como las ventajas y desventajas de los distintos algoritmos, queda fuera del ámbito de este tutorial. Para obtener una visión detallada de estos problemas, incluidas instrucciones sobre qué algoritmos de cifrado y validación usar, qué longitudes de clave usar y cómo generar estas claves de forma más adecuada, consulte *seguridad, pertenencia y administración de roles de Professional ASP.NET 2,0*.

De forma predeterminada, las claves utilizadas para el cifrado y la validación se generan automáticamente para cada aplicación, y estas claves se almacenan en la autoridad de seguridad local (LSA). En Resumen, la configuración predeterminada garantiza claves únicas en un servidor Web por servidor Web y aplicación a aplicación. Por lo tanto, este comportamiento predeterminado no funcionará en los dos escenarios siguientes:

- **Granjas** de servidores Web: en un escenario de [granja de servidores web](http://en.wikipedia.org/wiki/Web_farm) , una sola aplicación web se hospeda en varios servidores web para fines de escalabilidad y redundancia. Cada solicitud entrante se envía a un servidor de la granja, lo que significa que, a lo largo de la duración de la sesión de un usuario, se pueden usar distintos servidores para administrar sus diversas solicitudes. Por lo tanto, cada servidor debe usar las mismas claves de cifrado y validación para que el vale de autenticación de formularios creado, cifrado y validado en un servidor se pueda descifrar y validar en un servidor diferente de la granja.
- **Uso compartido de vales entre aplicaciones** : un único servidor web puede hospedar varias aplicaciones ASP.net. Si necesita que estas aplicaciones diferentes compartan un único vale de autenticación de formularios, es imperativo que sus claves de cifrado y validación coincidan.

Cuando se trabaja en una granja de servidores web o se comparten vales de autenticación entre aplicaciones en el mismo servidor, deberá configurar el elemento &lt;machineKey&gt; en las aplicaciones afectadas para que sus valores decryptionKey y validationKey coincidan.

Aunque ninguno de los escenarios anteriores se aplica a nuestra aplicación de ejemplo, todavía podemos especificar valores decryptionKey y validationKey explícitos y definir los algoritmos que se van a utilizar. Agregue un valor &lt;machineKey&gt; al archivo Web. config:

[!code-xml[Main](forms-authentication-configuration-and-advanced-topics-cs/samples/sample5.xml)]

Para obtener más información, consulte [Cómo: configurar machineKey en ASP.NET 2,0](https://msdn.microsoft.com/library/ms998288.aspx).

> [!NOTE]
> Los valores decryptionKey y validationKey se han tomado de la [Página Web de contraseñas perfectas](https://www.grc.com/passwords.htm)de [Steve Gibson](http://www.grc.com/stevegibson.htm), que genera 64 caracteres hexadecimales aleatorios en cada visita de página. Para reducir la probabilidad de que estas claves se conviertan en sus aplicaciones de producción, se recomienda reemplazar las claves anteriores con las generadas de forma aleatoria en la página contraseñas perfectas.

## <a name="step-4-storing-additional-user-data-in-the-ticket"></a>Paso 4: almacenamiento de datos de usuario adicionales en el vale

Muchas aplicaciones web muestran información acerca de la presentación de la página o la basa en el usuario que ha iniciado sesión actualmente. Por ejemplo, una página web podría mostrar el nombre del usuario y la fecha de la última vez que inició sesión en la esquina superior de cada página. El vale de autenticación de formularios almacena el nombre de usuario del usuario que ha iniciado sesión actualmente, pero cuando se necesita cualquier otra información, la página debe ir al almacén del usuario, normalmente una base de datos, para buscar la información no almacenada en el vale de autenticación.

Con un poco de código, se puede almacenar información de usuario adicional en el vale de autenticación de formularios. Estos datos se pueden expresar a través de la [propiedad UserData](https://msdn.microsoft.com/library/system.web.security.formsauthenticationticket.userdata.aspx)de la [clase FormsAuthenticationTicket](https://msdn.microsoft.com/library/system.web.security.formsauthenticationticket.aspx). Este es un lugar útil para colocar pequeñas cantidades de información sobre el usuario que se suele necesitar. El valor especificado en la propiedad UserData se incluye como parte de la cookie de vale de autenticación y, al igual que los demás campos de vale, se cifra y valida según la configuración del sistema de autenticación de formularios. De forma predeterminada, UserData es una cadena vacía.

Para almacenar los datos de usuario en el vale de autenticación, es necesario escribir un fragmento de código en la página de inicio de sesión que toma la información específica del usuario y la almacena en el vale. Puesto que UserData es una propiedad de tipo String, los datos almacenados en él deben serializarse correctamente como una cadena. Por ejemplo, Imagine que el almacén del usuario incluyó la fecha de nacimiento de cada usuario y el nombre de su empleador, y deseamos almacenar estos dos valores de propiedad en el vale de autenticación. Podríamos serializar estos valores en una cadena concatenando la fecha del usuario de la cadena de nacimiento con una barra vertical (|), seguida del nombre de la empresa. Para un usuario nacido el 15 de agosto de 1974 que funciona para Northwind Traders, asignaremos la propiedad UserData a la cadena: 1974-08-15 | Northwind Traders.

Siempre que sea necesario tener acceso a los datos almacenados en el vale, podemos hacerlo mediante la captura de la propiedad FormsAuthenticationTicket de la solicitud actual y la deserialización de la propiedad UserData. En el caso de la fecha de nacimiento y el ejemplo del nombre de la empresa, dividiremos la cadena UserData en dos subcadenas en función del delimitador (|).

[![se puede almacenar información de usuario adicional en el vale de autenticación](forms-authentication-configuration-and-advanced-topics-cs/_static/image11.png)](forms-authentication-configuration-and-advanced-topics-cs/_static/image10.png)

**Figura 04**: se puede almacenar información de usuario adicional en el vale de autenticación ([haga clic para ver la imagen de tamaño completo](forms-authentication-configuration-and-advanced-topics-cs/_static/image12.png))

### <a name="writing-information-to-userdata"></a>Escribir información en UserData

Desafortunadamente, agregar información específica del usuario a un vale de autenticación de formularios no es tan sencillo como cabría esperar. La propiedad UserData de la clase FormsAuthenticationTicket es de solo lectura y solo puede especificarse mediante el constructor de la clase FormsAuthenticationTicket. Al especificar la propiedad UserData en el constructor, también es necesario proporcionar los otros valores del vale: el nombre de usuario, la fecha del problema, la expiración, etc. Cuando creamos la página de inicio de sesión en el tutorial anterior, todo esto se trató en la clase FormsAuthentication. Al agregar UserData a FormsAuthenticationTicket, es necesario escribir código para replicar gran parte de la funcionalidad que ya proporciona la clase FormsAuthentication.

Vamos a explorar el código necesario para trabajar con UserData actualizando la página login. aspx para registrar información adicional sobre el usuario en el vale de autenticación. Supongamos que el almacén de usuarios contiene información sobre la empresa para la que el usuario trabaja y su título, y que queremos capturar esta información en el vale de autenticación. Actualice el controlador de eventos de clic de LoginButton de la página login. aspx para que el código tenga el aspecto siguiente:

[!code-csharp[Main](forms-authentication-configuration-and-advanced-topics-cs/samples/sample6.cs)]

Vamos a recorrer este código una línea cada vez. El método se inicia definiendo cuatro matrices de cadenas: usuarios, contraseñas, companyName y titleAtCompany. Estas matrices contienen los nombres de usuario, las contraseñas, los nombres de las empresas y los títulos de las cuentas de usuario del sistema, de las cuales hay tres: Scott, Jisun y Sam. En una aplicación real, estos valores se consultarían desde el almacén del usuario, no codificados de forma rígida en el código fuente de la página.

En el tutorial anterior, si las credenciales proporcionadas fueran válidas, simplemente se llamó a FormsAuthentication. RedirectFromLoginPage (UserName. Text, RememberMe. Checked), que llevó a cabo los pasos siguientes:

1. Se creó el vale de autenticación de formularios
2. Escribió el vale en el almacén adecuado. En el caso de los vales de autenticación basados en cookies, se usa la colección de cookies del explorador. en el caso de los vales de autenticación sin cookies, los datos del vale se serializan en la dirección URL.
3. Redirige al usuario a la página correspondiente

Estos pasos se replican en el código anterior. En primer lugar, la cadena que se almacenará finalmente en la propiedad UserData se forma combinando el nombre de la compañía y el título, delimitando los dos valores con un carácter de barra vertical (|).

String userDataString = cadena. Concat (companyName [i], "|", titleAtCompany [i]);

A continuación, se invoca el método FormsAuthentication. GetAuthCookie, que crea el vale de autenticación, lo cifra y valida según los valores de configuración y lo coloca en un objeto HttpCookie.

HttpCookie authCookie = FormsAuthentication. GetAuthCookie (UserName. Text, RememberMe. Checked);

Para trabajar con el FormAuthenticationTicket incrustado en la cookie, es necesario llamar al [método de descifrado](https://msdn.microsoft.com/library/system.web.security.formsauthentication.decrypt.aspx)de la clase FormAuthentication, pasando el valor de la cookie.

Vale de FormsAuthenticationTicket = FormsAuthentication. descifrado (authCookie. Value);

A continuación, creamos una *nueva* instancia de FormsAuthenticationTicket basada en los valores de FormsAuthenticationTicket existentes. Sin embargo, este nuevo vale incluye la información específica del usuario (userDataString).

FormsAuthenticationTicket newTicket = New FormsAuthenticationTicket (ticket. Versión, vale. Nombre, vale. IssueDate, vale. Expiración, vale. IsPersistent, userDataString);

Después, ciframos (y validamos) la nueva instancia de FormsAuthenticationTicket llamando al [método Encrypt](https://msdn.microsoft.com/library/system.web.security.formsauthentication.encrypt.aspx)y colocamos de nuevo estos datos cifrados (y validados) en authCookie.

authCookie. Value = FormsAuthentication. Encrypt (newTicket);

Por último, se agrega authCookie a la colección Response. cookies y se llama al método GetRedirectUrl para determinar la página adecuada para enviar al usuario.

[!code-csharp[Main](forms-authentication-configuration-and-advanced-topics-cs/samples/sample7.cs)]

Todo este código es necesario porque la propiedad UserData es de solo lectura y la clase FormsAuthentication no proporciona ningún método para especificar la información de UserData en sus métodos GetAuthCookie, SetAuthCookie o RedirectFromLoginPage.

> [!NOTE]
> El código que se acaba de examinar almacena información específica del usuario en un vale de autenticación basado en cookies. Las clases responsables de serializar el vale de autenticación de formularios en la dirección URL son internas al .NET Framework. La historia larga es breve, no se pueden almacenar los datos de usuario en un vale de autenticación de formularios sin cookies.

### <a name="accessing-the-userdata-information"></a>Obtener acceso a la información de UserData

En este punto, el nombre y el título de la empresa de cada usuario se almacenan en la propiedad UserData del vale de autenticación de formularios cuando inician sesión. Se puede tener acceso a esta información desde el vale de autenticación en cualquier página sin necesidad de un viaje al almacén del usuario. Para mostrar cómo se puede recuperar esta información de la propiedad UserData, vamos a actualizar default. aspx para que su mensaje de bienvenida incluya no solo el nombre del usuario, sino también la empresa para la que trabaja y su título.

Actualmente, default. aspx contiene un panel AuthenticatedMessagePanel con un control etiqueta denominado WelcomeBackMessage. Este panel solo se muestra a los usuarios autenticados. Actualice el código de la página default. aspx\_controlador de eventos LOAD para que tenga el aspecto siguiente:

[!code-csharp[Main](forms-authentication-configuration-and-advanced-topics-cs/samples/sample8.cs)]

Si request. IsAuthenticated es true, la propiedad Text de WelcomeBackMessage se establece primero en Welcome, *username*. A continuación, la propiedad User. Identity se convierte en un objeto FormsIdentity para que podamos tener acceso al FormsAuthenticationTicket subyacente. Una vez que tenemos el FormsAuthenticationTicket, deserializamos la propiedad UserData en el nombre y el título de la empresa. Esto se logra dividiendo la cadena en el carácter de barra vertical. El nombre y el título de la empresa se muestran en la etiqueta WelcomeBackMessage.

En la figura 5 se muestra una captura de pantalla de esta pantalla en acción. Al iniciar sesión como Scott se muestra un mensaje de bienvenida que incluye la empresa y el título de Scott.

[![se muestran la empresa y el título del usuario que ha iniciado sesión actualmente](forms-authentication-configuration-and-advanced-topics-cs/_static/image14.png)](forms-authentication-configuration-and-advanced-topics-cs/_static/image13.png)

**Figura 05**: se muestra la empresa y el título del usuario que ha iniciado sesión actualmente ([haga clic para ver la imagen de tamaño completo](forms-authentication-configuration-and-advanced-topics-cs/_static/image15.png))

> [!NOTE]
> La propiedad UserData del vale de autenticación actúa como una memoria caché para el almacén del usuario. Al igual que cualquier caché, debe actualizarse cuando se modifican los datos subyacentes. Por ejemplo, si hay una página web desde la que los usuarios pueden actualizar su perfil, los campos almacenados en la caché de la propiedad UserData deben actualizarse para reflejar los cambios realizados por el usuario.

## <a name="step-5-using-a-custom-principal"></a>Paso 5: uso de una entidad de seguridad personalizada

En cada solicitud entrante, FormsAuthenticationModule intenta autenticar al usuario. Si hay un vale de autenticación que no ha expirado, FormsAuthenticationModule asigna la propiedad HttpContext. User a un nuevo objeto GenericPrincipal. Este objeto GenericPrincipal tiene una identidad de tipo FormsIdentity, que incluye una referencia al vale de autenticación de formularios. La clase GenericPrincipal contiene la funcionalidad mínima mínima necesaria para una clase que implementa IPrincipal-it solo tiene una propiedad Identity y un método IsInRole.

El objeto principal tiene dos responsabilidades: para indicar los roles a los que pertenece el usuario y para proporcionar información de identidad. Esto se logra mediante el método IsInRole (*roleName*) de la interfaz de IPrincipal y la propiedad de identidad, respectivamente. La clase GenericPrincipal permite especificar una matriz de cadenas de nombres de rol a través de su constructor; su método IsInRole (*roleName*) simplemente comprueba si el argumento pasado en el valor de *roleName* existe dentro de la matriz de cadenas. Cuando FormsAuthenticationModule crea el GenericPrincipal, pasa una matriz de cadenas vacía al constructor de GenericPrincipal. Por consiguiente, cualquier llamada a IsInRole siempre devolverá FALSE.

La clase GenericPrincipal cumple las necesidades de la mayoría de los escenarios de autenticación basados en formularios donde no se usan los roles. En aquellas situaciones en las que el control de roles predeterminado es insuficiente o cuando es necesario asociar un objeto IIdentity personalizado al usuario, puede crear un objeto IPrincipal personalizado durante el flujo de trabajo de autenticación y asignarlo a la propiedad HttpContext. User.

> [!NOTE]
> Como veremos en futuros tutoriales, cuando ASP. El marco de trabajo de roles de la red está habilitado crea un objeto de entidad de seguridad personalizado de tipo [RolePrincipal](https://msdn.microsoft.com/library/system.web.security.roleprincipal.aspx) y sobrescribe el objeto GenericPrincipal creado por la autenticación de formularios. Para ello, se debe personalizar el método IsInRole de la entidad de seguridad para interactuar con la API del marco de trabajo de roles.

Puesto que aún no nos preocupamos por los roles, la única razón para crear una entidad de seguridad personalizada en este momento sería asociar un objeto IIdentity personalizado a la entidad de seguridad. En el paso 4, analizamos el almacenamiento de información de usuario adicional en la propiedad UserData del vale de autenticación, en particular, el nombre de la empresa del usuario y su título. Sin embargo, solo se puede tener acceso a la información de UserData a través del vale de autenticación y, a continuación, como una cadena serializada, lo que significa que siempre queremos ver la información de usuario almacenada en el vale que necesitamos para analizar la propiedad UserData.

Podemos mejorar la experiencia del desarrollador mediante la creación de una clase que implementa IIdentity e incluye las propiedades CompanyName y title. De este modo, un desarrollador puede tener acceso al nombre y el título de la empresa del usuario que ha iniciado sesión directamente a través de las propiedades CompanyName y title sin necesidad de saber cómo analizar la propiedad UserData.

### <a name="creating-the-custom-identity-and-principal-classes"></a>Crear las clases de identidad y entidad de seguridad personalizadas

En este tutorial, vamos a crear los objetos de entidad de seguridad y de identidad personalizados en la carpeta de código del\_de la aplicación. Comience agregando una carpeta de código de\_de aplicación al proyecto. Haga clic con el botón derecho en el nombre del proyecto en Explorador de soluciones, seleccione la opción Agregar carpeta ASP.NET y elija aplicación\_código. La carpeta de código del\_de la aplicación es una carpeta especial de ASP.NET que contiene archivos de clase específicos del sitio Web.

> [!NOTE]
> La carpeta de código del\_de la aplicación solo se debe usar al administrar el proyecto a través del modelo de proyecto del sitio Web. Si usa el modelo de [proyecto de aplicación web](https://msdn.microsoft.com/asp.net/Aa336618.aspx), cree una carpeta estándar y agregue las clases a ese. Por ejemplo, puede Agregar una nueva carpeta denominada clases y colocar el código allí.

A continuación, agregue dos nuevos archivos de clase a la carpeta de código del\_de la aplicación, uno denominado CustomIdentity.cs y otro denominado CustomPrincipal.cs.

[![agregar las clases CustomIdentity y CustomPrincipal al proyecto](forms-authentication-configuration-and-advanced-topics-cs/_static/image17.png)](forms-authentication-configuration-and-advanced-topics-cs/_static/image16.png)

**Figura 06**: adición de las clases CustomIdentity y CustomPrincipal al proyecto ([haga clic para ver la imagen de tamaño completo](forms-authentication-configuration-and-advanced-topics-cs/_static/image18.png))

La clase CustomIdentity es responsable de implementar la interfaz IIdentity, que define las propiedades AuthenticationType, IsAuthenticated y Name. Además de las propiedades requeridas, estamos interesados en exponer el vale de autenticación de formularios subyacente, así como las propiedades del nombre y el título de la empresa del usuario. Escriba el código siguiente en la clase CustomIdentity.

[!code-csharp[Main](forms-authentication-configuration-and-advanced-topics-cs/samples/sample9.cs)]

Tenga en cuenta que la clase incluye una variable miembro FormsAuthenticationTicket (vale\_) y que esta información de vale debe proporcionarse a través del constructor. Estos datos de vale se usan para devolver el nombre de la identidad. su propiedad UserData se analiza para devolver los valores de las propiedades CompanyName y title.

A continuación, cree la clase CustomPrincipal. Dado que no nos preocupa por los roles en este momento, el constructor de la clase CustomPrincipal solo acepta un objeto CustomIdentity. su método IsInRole siempre devuelve false.

[!code-csharp[Main](forms-authentication-configuration-and-advanced-topics-cs/samples/sample10.cs)]

### <a name="assigning-a-customprincipal-object-to-the-incoming-requests-security-context"></a>Asignar un objeto CustomPrincipal al contexto de seguridad de la solicitud entrante

Ahora tenemos una clase que extiende la especificación IIdentity predeterminada para incluir las propiedades CompanyName y title, así como una clase principal personalizada que usa la identidad personalizada. Estamos preparados para entrar en la canalización ASP.NET y asignar el objeto principal personalizado al contexto de seguridad de la solicitud entrante.

La canalización ASP.NET toma una solicitud entrante y la procesa a través de una serie de pasos. En cada paso, se genera un evento determinado, lo que permite a los desarrolladores puntear en la canalización ASP.NET y modificar la solicitud en determinados puntos del ciclo de vida. Por ejemplo, FormsAuthenticationModule espera a que ASP.NET genere el [evento AuthenticateRequest](https://msdn.microsoft.com/library/system.web.httpapplication.authenticaterequest.aspx), momento en el que inspecciona la solicitud entrante de un vale de autenticación. Si se encuentra un vale de autenticación, se crea un objeto GenericPrincipal y se asigna a la propiedad HttpContext. User.

Después del evento AuthenticateRequest, la canalización ASP.NET genera el [evento PostAuthenticateRequest](https://msdn.microsoft.com/library/system.web.httpapplication.postauthenticaterequest.aspx), que es donde podemos reemplazar el objeto GenericPrincipal creado por FormsAuthenticationModule con una instancia del objeto CustomPrincipal. En la ilustración 7 se muestra este flujo de trabajo.

[![el GenericPrincipal se sustituye por un CustomPrincipal en el evento PostAuthenticationRequest](forms-authentication-configuration-and-advanced-topics-cs/_static/image20.png)](forms-authentication-configuration-and-advanced-topics-cs/_static/image19.png)

**Figura 07**: el GenericPrincipal se sustituye por un CustomPrincipal en el evento PostAuthenticationRequest ([haga clic para ver la imagen a tamaño completo](forms-authentication-configuration-and-advanced-topics-cs/_static/image21.png))

Para ejecutar código en respuesta a un evento de canalización ASP.NET, podemos crear el controlador de eventos adecuado en global. asax o crear nuestro propio módulo HTTP. En este tutorial, vamos a crear el controlador de eventos en global. asax. Comience agregando global. asax a su sitio Web. Haga clic con el botón secundario en el nombre del proyecto en Explorador de soluciones y agregue un elemento de tipo clase de aplicación global denominada global. asax.

[![agregar un archivo global. asax a su sitio web](forms-authentication-configuration-and-advanced-topics-cs/_static/image23.png)](forms-authentication-configuration-and-advanced-topics-cs/_static/image22.png)

**Figura 08**: agregar un archivo global. asax al sitio web ([haga clic para ver la imagen de tamaño completo](forms-authentication-configuration-and-advanced-topics-cs/_static/image24.png))

La plantilla predeterminada global. asax incluye controladores de eventos para varios eventos de canalización ASP.NET, incluidos los eventos de inicio, finalización y [error](https://msdn.microsoft.com/library/system.web.httpapplication.error.aspx), entre otros. No dude en quitar estos controladores de eventos, ya que no los necesita para esta aplicación. El evento que nos interesa es PostAuthenticateRequest. Actualice el archivo global. asax para que su marcado tenga un aspecto similar al siguiente:

[!code-aspx[Main](forms-authentication-configuration-and-advanced-topics-cs/samples/sample11.aspx)]

La aplicación\_método OnPostAuthenticateRequest se ejecuta cada vez que el tiempo de ejecución de ASP.NET genera el evento PostAuthenticateRequest, que se produce una vez en cada solicitud de página entrante. El controlador de eventos se inicia comprobando si el usuario se autenticó y se autenticó mediante la autenticación de formularios. En ese caso, se crea un nuevo objeto CustomIdentity y se pasa el vale de autenticación de la solicitud actual en su constructor. A continuación, se crea un objeto CustomPrincipal y se pasa el objeto CustomIdentity que acaba de crear en su constructor. Por último, el contexto de seguridad de la solicitud actual se asigna al objeto CustomPrincipal recién creado.

Tenga en cuenta que el último paso: asociar el objeto CustomPrincipal con el contexto de seguridad de la solicitud: asigna la entidad de seguridad a dos propiedades: HttpContext. User y Thread. CurrentPrincipal. Estas dos asignaciones son necesarias debido a la manera en que se administran los contextos de seguridad en ASP.NET. El .NET Framework asocia un contexto de seguridad a cada subproceso en ejecución; Esta información está disponible como un objeto IPrincipal a través de la [propiedad CurrentPrincipal](https://msdn.microsoft.com/library/system.threading.thread.currentcontext.aspx)del [objeto Thread](https://msdn.microsoft.com/library/system.threading.thread.aspx). Lo que es confuso es que ASP.NET tiene su propia información de contexto de seguridad (HttpContext. User).

En ciertos escenarios, se examina la propiedad Thread. CurrentPrincipal al determinar el contexto de seguridad; en otros escenarios, se usa HttpContext. User. Por ejemplo, hay características de seguridad en .NET que permiten a los desarrolladores indicar de forma declarativa qué usuarios o roles pueden crear instancias de una clase o invocar métodos específicos (consulte [agregar reglas de autorización a las capas de negocio y de datos mediante PrincipalPermissionAttributes](https://weblogs.asp.net/scottgu/archive/2006/10/04/Tip_2F00_Trick_3A00_-Adding-Authorization-Rules-to-Business-and-Data-Layers-using-PrincipalPermissionAttributes.aspx)). En segundo plano, estas técnicas declarativas determinan el contexto de seguridad a través de la propiedad Thread. CurrentPrincipal.

En otros escenarios, se usa la propiedad HttpContext. User. Por ejemplo, en el tutorial anterior usamos esta propiedad para mostrar el nombre de usuario del usuario que ha iniciado sesión actualmente. Claramente, es imperativo que la información de contexto de seguridad en las propiedades Thread. CurrentPrincipal y HttpContext. User coincidan.

El tiempo de ejecución de ASP.NET sincroniza automáticamente estos valores de propiedad. Sin embargo, esta sincronización se produce después del evento AuthenticateRequest, pero *antes* del evento PostAuthenticateRequest. Por lo tanto, al agregar una entidad de seguridad personalizada en el evento PostAuthenticateRequest, debemos estar seguro de asignar manualmente Thread. CurrentPrincipal o else Thread. CurrentPrincipal y HttpContext. User estarán fuera de sincronización. Vea [context. User frente a Thread. CurrentPrincipal](http://leastprivilege.com/2005/11/23/context-user-vs-thread-currentprincipal/) para obtener una explicación más detallada sobre este problema.

### <a name="accessing-the-companyname-and-title-properties"></a>Acceder a las propiedades CompanyName y title

Siempre que llega una solicitud y se envía al motor ASP.NET, se activará la aplicación\_controlador de eventos OnPostAuthenticateRequest en global. asax. Si la solicitud se ha autenticado correctamente mediante FormsAuthenticationModule, el controlador de eventos creará un nuevo objeto CustomPrincipal con un objeto CustomIdentity basado en el vale de autenticación de formularios. Con esta lógica implementada, el acceso a la información sobre el nombre y el título de la empresa del usuario que ha iniciado sesión actualmente es increíblemente sencillo.

Vuelva a la página\_controlador de eventos de carga en default. aspx, donde en el paso 4 hemos escrito código para recuperar el vale de autenticación del formulario y analizar la propiedad UserData para mostrar el nombre y el título de la empresa del usuario. Con los objetos CustomPrincipal y CustomIdentity en uso ahora, no es necesario analizar los valores de la propiedad UserData del vale. En su lugar, solo tiene que obtener una referencia al objeto CustomIdentity y usar sus propiedades CompanyName y title:

[!code-csharp[Main](forms-authentication-configuration-and-advanced-topics-cs/samples/sample12.cs)]

## <a name="summary"></a>Resumen

En este tutorial, hemos examinado cómo personalizar la configuración del sistema de autenticación de formularios a través de Web. config. Analizamos cómo se controla la expiración del vale de autenticación y cómo se usan las medidas de seguridad de cifrado y validación para proteger el vale de la inspección y modificación. Por último, analizamos el uso de la propiedad UserData del vale de autenticación para almacenar información de usuario adicional en el vale, y cómo usar los objetos de entidad de seguridad y de identidad personalizados para exponer esta información de forma más fácil de usar.

En este tutorial concluye nuestro examen de la autenticación de formularios en ASP.NET. En el siguiente tutorial se inicia nuestro viaje en el marco de pertenencia.

¡ Feliz programación!

### <a name="further-reading"></a>Información adicional

Para obtener más información sobre los temas descritos en este tutorial, consulte los siguientes recursos:

- [Dessección de la autenticación de formularios](http://aspnet.4guysfromrolla.com/articles/072005-1.aspx)
- [Explicación: autenticación mediante formularios en ASP.NET 2,0](https://msdn.microsoft.com/library/aa480476.aspx)
- [Cómo: proteger la autenticación de formularios en ASP.NET 2,0](https://msdn.microsoft.com/library/ms998310.aspx)
- [Seguridad, pertenencia y administración de roles de Professional ASP.NET 2,0](http://www.wrox.com/WileyCDA/WroxTitle/productCd-0764596985.html) (ISBN: 978-0-7645-9698-8)
- [Proteger los controles de inicio de sesión](https://msdn.microsoft.com/library/ms178346.aspx)
- [El elemento&gt; de autenticación de &lt;](https://msdn.microsoft.com/library/532aee0e.aspx)
- [El elemento de&gt; de &lt;Forms para la autenticación de &lt;&gt;](https://msdn.microsoft.com/library/1d3t3c61.aspx)
- [El elemento &lt;machineKey&gt;](https://msdn.microsoft.com/library/w8h3skw9.aspx)
- [Descripción del vale de autenticación de formularios y cookie](https://support.microsoft.com/kb/910443)

### <a name="video-training-on-topics-contained-in-this-tutorial"></a>Vídeo de aprendizaje sobre temas incluidos en este tutorial

- [Cómo cambiar las propiedades de autenticación de formularios](../../../videos/authentication/how-to-change-the-forms-authentication-properties.md)
- [Cómo configurar y usar la autenticación sin cookies en una aplicación ASP.NET](../../../videos/authentication/how-to-setup-and-use-cookie-less-authentication-in-an-aspnet-application.md)
- [Reubicación del inicio de sesión de los formularios ASP](../../../videos/authentication/asp-forms-login-relocation.md)
- [Configuración de claves personalizadas de inicio de sesión de los formularios](../../../videos/authentication/forms-login-custom-key-configuration.md)
- [Agregar datos personalizados al método de autenticación](../../../videos/authentication/add-custom-data-to-the-authentication-method.md)
- [Usar objetos de entidad de seguridad personalizados](../../../videos/authentication/use-custom-principal-objects.md)

### <a name="about-the-author"></a>Acerca del autor

Scott Mitchell, autor de varios libros de ASP/ASP. NET y fundador de 4GuysFromRolla.com, ha estado trabajando con las tecnologías Web de Microsoft desde 1998. Scott funciona como consultor, profesor y redactor independiente. Su último libro se *[enseña a ASP.NET 2,0 en 24 horas](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)* . Se puede acceder a Scott en [mitchell@4guysfromrolla.com](mailto:mitchell@4guysfromrolla.com) o a través de su blog en [http://ScottOnWriting.NET](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Agradecimiento especial a

Muchos revisores útiles revisaron esta serie de tutoriales. El revisor del cliente potencial de este tutorial fue Alicja Maziarz. ¿Está interesado en revisar los próximos artículos de MSDN? En caso afirmativo, suéltelo en [mitchell@4GuysFromRolla.com](mailto:mitchell@4guysfromrolla.com).

> [!div class="step-by-step"]
> [Anterior](an-overview-of-forms-authentication-cs.md)
> [Siguiente](security-basics-and-asp-net-support-vb.md)
