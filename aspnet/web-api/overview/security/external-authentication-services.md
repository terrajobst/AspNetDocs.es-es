---
uid: web-api/overview/security/external-authentication-services
title: Servicios de autenticación externos con ASP.NET Web APIC#() | Microsoft Docs
author: rmcmurray
description: Describe el uso de servicios de autenticación externos en ASP.NET Web API.
ms.author: riande
ms.date: 01/28/2019
ms.assetid: 3bb8eb15-b518-44f5-a67d-a27e051aedc6
msc.legacyurl: /web-api/overview/security/external-authentication-services
msc.type: authoredcontent
ms.openlocfilehash: b2571552a3f8040ff42bfa0a9fa48981f71a1e4b
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78447421"
---
# <a name="external-authentication-services-with-aspnet-web-api-c"></a>Servicios de autenticación externos con ASP.NET Web APIC#()

Visual Studio 2017 y ASP.NET 4.7.2 expanden las opciones de seguridad para [aplicaciones de una sola página](../../../single-page-application/index.md) (Spa) y servicios de [API Web](../../index.md) para la integración con servicios de autenticación externos, entre los que se incluyen varios servicios de autenticación de redes sociales y de OAuth/OpenID: Microsoft Accounts, Twitter, Facebook y Google.  

### <a name="in-this-walkthrough"></a>En este tutorial

- [Usar servicios de autenticación externos](#USING)
- [Crear la aplicación Web de ejemplo](#SAMPLE)
- [Habilitación de la autenticación de Facebook](#FACEBOOK)
- [Habilitar la autenticación de Google](#GOOGLE)
- [Habilitar la autenticación de Microsoft](#MICROSOFT)
- [Habilitación de la autenticación de Twitter](#TWITTER)
- [Información adicional](#MOREINFO)

    - [Combinación de servicios de autenticación externos](#COMBINE)
    - [Configuración de IIS Express para utilizar un nombre de dominio completo](#FQDN)
    - [Cómo obtener la configuración de la aplicación para la autenticación de Microsoft](#OBTAIN)
    - [Opcional: deshabilitar el registro local](#DISABLE)

### <a name="prerequisites"></a>Requisitos previos

Para seguir los ejemplos de este tutorial, debe disponer de lo siguiente:

- Visual Studio 2017
- Una cuenta de desarrollador con el identificador de la aplicación y la clave secreta para uno de los siguientes servicios de autenticación de medios sociales:

  - Cuentas de Microsoft ([https://go.microsoft.com/fwlink/?LinkID=144070](https://go.microsoft.com/fwlink/?LinkID=144070))
  - Twitter ([https://dev.twitter.com/](https://dev.twitter.com/))
  - Facebook ([https://developers.facebook.com/](https://developers.facebook.com/))
  - Google ([https://developers.google.com/](https://developers.google.com))

<a id="USING"></a>
## <a name="using-external-authentication-services"></a>Usar servicios de autenticación externos

La abundancia de servicios de autenticación externos que actualmente están disponibles para los desarrolladores web ayuda a reducir el tiempo de desarrollo al crear nuevas aplicaciones Web. Los usuarios Web suelen tener varias cuentas existentes para los sitios web de servicios web y medios sociales más conocidos, por lo que cuando una aplicación web implementa los servicios de autenticación desde un servicio Web externo o un sitio web de redes sociales, ahorra el tiempo de desarrollo que se habría dedicado a crear una implementación de autenticación. El uso de un servicio de autenticación externo evita que los usuarios finales tengan que crear otra cuenta para la aplicación web y que también no tengan que recordar otro nombre de usuario y contraseña.

En el pasado, los desarrolladores tenían dos opciones: crear su propia implementación de autenticación u obtener información sobre cómo integrar un servicio de autenticación externo en sus aplicaciones. En el nivel más básico, el siguiente diagrama muestra un flujo de solicitud simple para un agente de usuario (explorador Web) que solicita información de una aplicación web que está configurada para usar un servicio de autenticación externo:

[![](external-authentication-services/_static/image2.png "Click to Expand the Image")](external-authentication-services/_static/image1.png)

En el diagrama anterior, el agente de usuario (o el explorador Web en este ejemplo) realiza una solicitud a una aplicación Web, que redirige el explorador Web a un servicio de autenticación externo. El agente de usuario envía sus credenciales al servicio de autenticación externo y, si el agente de usuario se ha autenticado correctamente, el servicio de autenticación externo redirigirá al agente de usuario a la aplicación web original con algún tipo de token que el el agente de usuario enviará a la aplicación Web. La aplicación web usará el token para comprobar que el agente de usuario se ha autenticado correctamente mediante el servicio de autenticación externo, y la aplicación web puede usar el token para recopilar más información sobre el agente de usuario. Una vez que la aplicación termina de procesar la información del agente de usuario, la aplicación web devolverá la respuesta adecuada al agente de usuario en función de su configuración de autorización.

En este segundo ejemplo, el agente de usuario negocia con la aplicación web y el servidor de autorización externo, y la aplicación web realiza una comunicación adicional con el servidor de autorización externo para recuperar información adicional sobre el usuario. directivas

[![](external-authentication-services/_static/image4.png "Click to Expand the Image")](external-authentication-services/_static/image3.png)

Visual Studio 2017 y ASP.NET 4.7.2 facilitan la integración con servicios de autenticación externos para los desarrolladores, ya que proporcionan una integración integrada para los siguientes servicios de autenticación:

- Facebook
- Google
- Cuentas de Microsoft (cuentas de Windows Live ID)
- Twitter

En los ejemplos de este tutorial se muestra cómo configurar cada uno de los servicios de autenticación externa admitidos mediante la nueva plantilla de aplicación Web de ASP.NET que se incluye con Visual Studio 2017.

> [!NOTE]
> Si es necesario, puede que tenga que agregar su FQDN a la configuración del servicio de autenticación externo. Este requisito se basa en las restricciones de seguridad de algunos servicios de autenticación externos que requieren que el FQDN de la configuración de la aplicación coincida con el FQDN que usan los clientes. (Los pasos para ello variarán en gran medida para cada servicio de autenticación externo; tendrá que consultar la documentación de cada servicio de autenticación externo para ver si es necesario y cómo configurar estos valores). Si necesita configurar IIS Express para usar un FQDN para probar este entorno, vea la sección [configuración de IIS Express para usar un nombre de dominio completo](#FQDN) más adelante en este tutorial.

<a id="SAMPLE"></a>
## <a name="create-a-sample-web-application"></a>Crear una aplicación Web de ejemplo

Los pasos siguientes le guiarán en el proceso de creación de una aplicación de ejemplo con la plantilla de aplicación Web ASP.NET y la utilizará para cada uno de los servicios de autenticación externos más adelante en este tutorial.

Inicie Visual Studio 2017 y seleccione **nuevo proyecto** en la página de inicio. O bien, en el menú **archivo** , seleccione **nuevo** y luego **proyecto**.

<!-- [![](external-authentication-services/_static/image6.png "Click to Expand the Image")](external-authentication-services/_static/image5.png) -->

Cuando se muestre el cuadro de diálogo **nuevo proyecto** , seleccione **instalado** y expanda **Visual C#** . En **Visual C#** , seleccione **Web**. En la lista de plantillas de proyecto, seleccione **aplicación Web de ASP.net (.NET Framework)** . Escriba un nombre para el proyecto y haga clic en **Aceptar**.

[![](external-authentication-services/_static/image71.png "Click to Expand the Image")](external-authentication-services/_static/image71.png)

Cuando se muestre el **nuevo proyecto ASP.net** , seleccione la plantilla **aplicación de una sola página** y haga clic en **crear proyecto**.

[![](external-authentication-services/_static/image72.png "Click to Expand the Image")](external-authentication-services/_static/image72.png)

Espere mientras Visual Studio 2017 crea el proyecto.

<!-- [![](external-authentication-services/_static/image12.png "Click to Expand the Image")](external-authentication-services/_static/image11.png) -->

Cuando Visual Studio 2017 haya terminado de crear el proyecto, abra el archivo *Startup.auth.CS* que se encuentra en la **aplicación\_** carpeta de inicio.

La primera vez que se crea el proyecto, ninguno de los servicios de autenticación externos está habilitado en el archivo *Startup.auth.CS* ; a continuación se muestra el aspecto que podría tener el código, con las secciones resaltadas para donde habilitaría un servicio de autenticación externo y cualquier configuración relevante para usar cuentas Microsoft, Twitter, Facebook o la autenticación de Google con la aplicación ASP.NET:

[!code-csharp[Main](external-authentication-services/samples/sample1.cs)]

Al presionar F5 para compilar y depurar la aplicación Web, se mostrará una pantalla de inicio de sesión donde verá que no se ha definido ningún servicio de autenticación externo.

[![](external-authentication-services/_static/image73.png "Click to Expand the Image")](external-authentication-services/_static/image73.png)

En las secciones siguientes, obtendrá información sobre cómo habilitar cada uno de los servicios de autenticación externos que se proporcionan con ASP.NET en Visual Studio 2017.

<a id="FACEBOOK"></a>
## <a name="enabling-facebook-authentication"></a>Habilitación de la autenticación de Facebook

El uso de la autenticación de Facebook requiere la creación de una cuenta de desarrollador de Facebook y el proyecto requerirá un identificador de aplicación y una clave secreta de Facebook para funcionar. Para obtener información sobre cómo crear una cuenta de desarrollador de Facebook y obtener el identificador de la aplicación y la clave secreta, consulte [https://go.microsoft.com/fwlink/?LinkID=252166](https://go.microsoft.com/fwlink/?LinkID=252166).

Una vez que haya obtenido el identificador y la clave secreta de la aplicación, siga estos pasos para habilitar la autenticación de Facebook para la aplicación web:

1. Cuando el proyecto esté abierto en Visual Studio 2017, abra el archivo *Startup.auth.CS* .

2. Busque la sección de autenticación de Facebook en el código:

    [!code-csharp[Main](external-authentication-services/samples/sample2.cs)]
3. Quite el &quot;//&quot; caracteres para quitar las marcas de comentario de las líneas de código resaltadas y, a continuación, agregue el identificador de aplicación y la clave secreta. Una vez que haya agregado esos parámetros, puede volver a compilar el proyecto:

    [!code-csharp[Main](external-authentication-services/samples/sample3.cs)]
4. Al presionar F5 para abrir la aplicación web en el explorador Web, verá que Facebook se ha definido como un servicio de autenticación externo:

    [![](external-authentication-services/_static/image74.png "Click to Expand the Image")](external-authentication-services/_static/image74.png)
5. Al hacer clic en el botón de **Facebook** , el explorador se redirigirá a la página de inicio de sesión de Facebook:

    [![](external-authentication-services/_static/image22.png "Click to Expand the Image")](external-authentication-services/_static/image21.png)
6. Después de escribir sus credenciales de Facebook y hacer clic en **iniciar sesión**, el explorador Web se redirigirá de nuevo a la aplicación Web, lo que le pedirá el **nombre de usuario** que desea asociar a su cuenta de Facebook:

    [![](external-authentication-services/_static/image24.png "Click to Expand the Image")](external-authentication-services/_static/image23.png)
7. Después de escribir el nombre de usuario y hacer clic en el botón de **registro** , la aplicación web mostrará la **Página principal** predeterminada de la cuenta de Facebook:

    [![](external-authentication-services/_static/image26.png "Click to Expand the Image")](external-authentication-services/_static/image25.png)

<a id="GOOGLE"></a>
## <a name="enabling-google-authentication"></a>Habilitar la autenticación de Google

El uso de la autenticación de Google requiere la creación de una cuenta de desarrollador de Google, y el proyecto requerirá un identificador de aplicación y una clave secreta de Google para poder funcionar. Para obtener información sobre cómo crear una cuenta de desarrollador de Google y obtener el identificador de la aplicación y la clave secreta, consulte [https://developers.google.com](https://developers.google.com).

Para habilitar la autenticación de Google para la aplicación Web, siga estos pasos:

1. Cuando el proyecto esté abierto en Visual Studio 2017, abra el archivo *Startup.auth.CS* .

2. Busque la sección de código de la autenticación de Google:

    [!code-csharp[Main](external-authentication-services/samples/sample4.cs)]
3. Quite el &quot;//&quot; caracteres para quitar las marcas de comentario de las líneas de código resaltadas y, a continuación, agregue el identificador de aplicación y la clave secreta. Una vez que haya agregado esos parámetros, puede volver a compilar el proyecto:

    [!code-csharp[Main](external-authentication-services/samples/sample5.cs)]
4. Al presionar F5 para abrir la aplicación web en el explorador Web, verá que Google se ha definido como un servicio de autenticación externo:

    [![](external-authentication-services/_static/image75.png "Click to Expand the Image")](external-authentication-services/_static/image75.png)
5. Al hacer clic en el botón de **Google** , el explorador se redirigirá a la página de inicio de sesión de Google:

    [![](external-authentication-services/_static/image32.png "Click to Expand the Image")](external-authentication-services/_static/image31.png)
6. Después de escribir sus credenciales de Google y hacer clic en **iniciar sesión**, Google le pedirá que compruebe que la aplicación web tiene permisos para acceder a su cuenta de Google:

    [![](external-authentication-services/_static/image34.png "Click to Expand the Image")](external-authentication-services/_static/image33.png)
7. Al hacer clic en **Aceptar**, el explorador Web se redirigirá de nuevo a la aplicación Web, lo que le pedirá el **nombre de usuario** que desea asociar a su cuenta de Google:

    [![](external-authentication-services/_static/image36.png "Click to Expand the Image")](external-authentication-services/_static/image35.png)
8. Después de escribir el nombre de usuario y hacer clic en el botón de **registro** , la aplicación web mostrará la **Página principal** predeterminada de la cuenta de Google:

    [![](external-authentication-services/_static/image38.png "Click to Expand the Image")](external-authentication-services/_static/image37.png)

<a id="MICROSOFT"></a>
## <a name="enabling-microsoft-authentication"></a>Habilitar la autenticación de Microsoft

La autenticación de Microsoft requiere la creación de una cuenta de desarrollador y requiere un identificador de cliente y un secreto de cliente para poder funcionar. Para obtener información sobre cómo crear una cuenta de desarrollador de Microsoft y obtener el identificador de cliente y el secreto de cliente, consulte [https://go.microsoft.com/fwlink/?LinkID=144070](https://go.microsoft.com/fwlink/?LinkID=144070).

Una vez que haya obtenido la clave de consumidor y el secreto de consumidor, siga estos pasos para habilitar la autenticación de Microsoft para su aplicación web:

1. Cuando el proyecto esté abierto en Visual Studio 2017, abra el archivo *Startup.auth.CS* .

2. Busque la sección de código de autenticación de Microsoft:

    [!code-csharp[Main](external-authentication-services/samples/sample6.cs)]
3. Quite el &quot;//&quot; caracteres para quitar las marcas de comentario de las líneas de código resaltadas y, a continuación, agregue el identificador de cliente y el secreto de cliente. Una vez que haya agregado esos parámetros, puede volver a compilar el proyecto:

    [!code-csharp[Main](external-authentication-services/samples/sample7.cs)]
4. Al presionar F5 para abrir la aplicación web en el explorador Web, verá que Microsoft se ha definido como un servicio de autenticación externo:

    [![](external-authentication-services/_static/image42.png "Click to Expand the Image")](external-authentication-services/_static/image41.png)
5. Al hacer clic en el botón **Microsoft** , el explorador se redirigirá a la página de inicio de sesión de Microsoft:

    [![](external-authentication-services/_static/image44.png "Click to Expand the Image")](external-authentication-services/_static/image43.png)
6. Después de escribir sus credenciales de Microsoft y hacer clic en **iniciar sesión**, se le pedirá que compruebe que la aplicación web tiene permisos de acceso a la cuenta de Microsoft:

    [![](external-authentication-services/_static/image46.png "Click to Expand the Image")](external-authentication-services/_static/image45.png)
7. Al hacer clic en **sí**, el explorador Web se redirigirá de nuevo a la aplicación Web, lo que le pedirá el **nombre de usuario** que desea asociar a su cuenta de Microsoft:

    [![](external-authentication-services/_static/image48.png "Click to Expand the Image")](external-authentication-services/_static/image47.png)
8. Después de escribir el nombre de usuario y hacer clic en el botón de **registro** , la aplicación web mostrará la **Página principal** predeterminada del cuenta de Microsoft:

    [![](external-authentication-services/_static/image50.png "Click to Expand the Image")](external-authentication-services/_static/image49.png)

<a id="TWITTER"></a>
## <a name="enabling-twitter-authentication"></a>Habilitación de la autenticación de Twitter

La autenticación de Twitter requiere la creación de una cuenta de desarrollador y requiere una clave de consumidor y un secreto de consumidor para poder funcionar. Para obtener información sobre cómo crear una cuenta de desarrollador de Twitter y obtener la clave de consumidor y el secreto de consumidor, consulte [https://go.microsoft.com/fwlink/?LinkID=252166](https://go.microsoft.com/fwlink/?LinkID=252166).

Una vez que haya obtenido la clave de consumidor y el secreto de consumidor, siga estos pasos para habilitar la autenticación de Twitter para la aplicación web:

1. Cuando el proyecto esté abierto en Visual Studio 2017, abra el archivo *Startup.auth.CS* .

2. Busque la sección autenticación de Twitter del código:

    [!code-csharp[Main](external-authentication-services/samples/sample8.cs)]
3. Quite el &quot;//&quot; caracteres para quitar la marca de comentario de las líneas de código resaltadas y, a continuación, agregue la clave de consumidor y el secreto de consumidor. Una vez que haya agregado esos parámetros, puede volver a compilar el proyecto:

    [!code-csharp[Main](external-authentication-services/samples/sample9.cs)]
4. Al presionar F5 para abrir la aplicación web en el explorador Web, verá que Twitter se ha definido como un servicio de autenticación externo:

    [![](external-authentication-services/_static/image54.png "Click to Expand the Image")](external-authentication-services/_static/image53.png)
5. Al hacer clic en el botón **Twitter** , el explorador se redirigirá a la página de inicio de sesión de Twitter:

    [![](external-authentication-services/_static/image56.png "Click to Expand the Image")](external-authentication-services/_static/image55.png)
6. Después de escribir sus credenciales de Twitter y hacer clic en **autorizar aplicación**, el explorador Web se redirigirá de nuevo a la aplicación Web, lo que le pedirá el **nombre de usuario** que quiere asociar a su cuenta de Twitter:

    [![](external-authentication-services/_static/image58.png "Click to Expand the Image")](external-authentication-services/_static/image57.png)
7. Después de escribir el nombre de usuario y hacer clic en el botón de **registro** , la aplicación web mostrará la **Página principal** predeterminada de la cuenta de Twitter:

    [![](external-authentication-services/_static/image60.png "Click to Expand the Image")](external-authentication-services/_static/image59.png)

<a id="MOREINFO"></a>
## <a name="additional-information"></a>Información adicional

Para obtener información adicional sobre la creación de aplicaciones que usan OAuth y OpenID, consulte las direcciones URL siguientes:

- [https://go.microsoft.com/fwlink/?LinkID=252166](https://go.microsoft.com/fwlink/?LinkID=252166)
- [https://go.microsoft.com/fwlink/?LinkID=243995](https://go.microsoft.com/fwlink/?LinkID=243995)

<a id="COMBINE"></a>
### <a name="combining-external-authentication-services"></a>Combinación de servicios de autenticación externos

Para mayor flexibilidad, puede definir varios servicios de autenticación externos al mismo tiempo, lo que permite a los usuarios de la aplicación web usar una cuenta de cualquiera de los servicios de autenticación externa habilitados:

[![](external-authentication-services/_static/image62.png "Click to Expand the Image")](external-authentication-services/_static/image61.png)

<a id="FQDN"></a>
### <a name="configure-iis-express-to-use-a-fully-qualified-domain-name"></a>Configurar IIS Express para usar un nombre de dominio completo

Algunos proveedores de autenticación externos no admiten la prueba de la aplicación mediante una dirección HTTP como `http://localhost:port/`. Para solucionar este problema, puede Agregar una asignación de nombre de dominio completo (FQDN) estática al archivo de hosts y configurar las opciones del proyecto en Visual Studio 2017 para usar el FQDN para pruebas o depuración. Para ello, siga estos pasos:

- Agregue un FQDN estático que asigne el archivo de hosts:

  1. Abra un símbolo del sistema con privilegios elevados en Windows.
  2. Escriba el comando siguiente:

      <kbd>Bloc de notas%WinDir%\system32\drivers\etc\hosts</kbd>
  3. Agregue una entrada como la siguiente al archivo de hosts:

      <kbd>127.0.0.1 www.wingtiptoys.com</kbd>
  4. Guarde y cierre el archivo de hosts.

- Configure el proyecto de Visual Studio para usar el FQDN:

  1. Cuando el proyecto esté abierto en Visual Studio 2017, haga clic en el menú **proyecto** y, a continuación, seleccione las propiedades del proyecto. Por ejemplo, puede seleccionar **propiedades de WebApplication1**.
  2. Seleccione la pestaña **Web** .
  3. Escriba el FQDN de la <strong>dirección URL del proyecto</strong>. Por ejemplo, debe especificar <kbd><http://www.wingtiptoys.com></kbd> si se trata de la asignación de FQDN que ha agregado al archivo de hosts.

- Configure IIS Express para usar el FQDN de la aplicación:

    1. Abra un símbolo del sistema con privilegios elevados en Windows.
    2. Escriba el siguiente comando para cambiar a la carpeta IIS Express:

        <kbd>CD/d &quot;%ProgramFiles%\IIS Express&quot;</kbd>
    3. Escriba el siguiente comando para agregar el FQDN a la aplicación:

        <kbd>appcmd. exe set config-Section: System. applicationHost/sites/+&quot;[name = ' WebApplication1 ']. bindings. [Protocol = ' http ', bindingInformation = ' *: 80: www. wingtiptoys. com ']&quot;/commit: apphost</kbd>

  Donde **WebApplication1** es el nombre del proyecto y **bindingInformation** contiene el número de puerto y el FQDN que desea usar para las pruebas.

<a id="OBTAIN"></a>
### <a name="how-to-obtain-your-application-settings-for-microsoft-authentication"></a>Cómo obtener la configuración de la aplicación para la autenticación de Microsoft

La vinculación de una aplicación a Windows Live para la autenticación de Microsoft es un proceso sencillo. Si aún no ha vinculado una aplicación a Windows Live, puede seguir estos pasos:

1. Vaya a [https://go.microsoft.com/fwlink/?LinkID=144070](https://go.microsoft.com/fwlink/?LinkID=144070) y escriba el nombre de cuenta de Microsoft y la contraseña cuando se le solicite y, a continuación, haga clic en **iniciar sesión**:

   <!--  [![](external-authentication-services/_static/image64.png "Click to Expand the Image")](external-authentication-services/_static/image63.png) -->
2. Seleccione **Agregar una aplicación** , escriba el nombre de la aplicación cuando se le solicite y, a continuación, haga clic en **crear**:

    [![](external-authentication-services/_static/image79.png "Click to Expand the Image")](external-authentication-services/_static/image79.png)
3. Seleccione la aplicación en **nombre** y aparece la página Propiedades de la aplicación.

4. Escriba el dominio de redirección de la aplicación. Copie el **identificador** de la aplicación y, en **secretos de aplicación**, seleccione **generar contraseña**. Copie la contraseña que aparece. El identificador y la contraseña de la aplicación son el identificador de cliente y el secreto de cliente. Seleccione **Aceptar** y, a continuación, **Guardar**.

    [![](external-authentication-services/_static/image77.png "Click to Expand the Image")](external-authentication-services/_static/image77.png)

<a id="DISABLE"></a>
### <a name="optional-disable-local-registration"></a>Opcional: deshabilitar el registro local

La funcionalidad de registro local de ASP.NET actual no impide que los programas automáticos (bots) creen cuentas de miembro; por ejemplo, mediante el uso de una tecnología de prevención y validación de bot, como [CAPTCHA](../../../web-pages/overview/security/16-adding-security-and-membership.md). Por ello, debe quitar el formulario de inicio de sesión local y el vínculo de registro en la página de inicio de sesión. Para ello, abra la página *\_login. cshtml* del proyecto y, a continuación, comente las líneas del panel de inicio de sesión local y el vínculo de registro. La página resultante debe ser similar a la del ejemplo de código siguiente:

[!code-html[Main](external-authentication-services/samples/sample10.html)]

Una vez deshabilitado el panel de inicio de sesión local y el vínculo de registro, la página de inicio de sesión solo mostrará los proveedores de autenticación externos que ha habilitado:

[![](external-authentication-services/_static/image70.png "Click to Expand the Image")](external-authentication-services/_static/image69.png)
