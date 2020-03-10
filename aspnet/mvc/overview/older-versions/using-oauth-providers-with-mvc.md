---
uid: mvc/overview/older-versions/using-oauth-providers-with-mvc
title: Uso de proveedores de OAuth con MVC 4 | Microsoft Docs
author: Rick-Anderson
description: En este tutorial se muestra cómo compilar una aplicación Web de ASP.NET MVC 4 que permite a los usuarios iniciar sesión con credenciales de un proveedor externo, como Facebo...
ms.author: riande
ms.date: 06/19/2013
ms.assetid: 7a87f16f-0e19-4f15-a88a-094ae866c4a2
msc.legacyurl: /mvc/overview/older-versions/using-oauth-providers-with-mvc
msc.type: authoredcontent
ms.openlocfilehash: 5dfd1305376a62f4987caea242ca0f6aac1018e9
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78433369"
---
# <a name="using-oauth-providers-with-mvc-4"></a>Usar proveedores OAuth con MVC 4

por [Tom FitzMacken](https://github.com/tfitzmac)

> En este tutorial se muestra cómo compilar una aplicación Web de ASP.NET MVC 4 que permite a los usuarios iniciar sesión con credenciales de un proveedor externo, como Facebook, Twitter, Microsoft o Google, y, a continuación, integrar parte de la funcionalidad de esos proveedores en el aplicación Web. Para simplificar, este tutorial se centra en trabajar con las credenciales de Facebook.
> 
> Para usar credenciales externas en una aplicación web MVC 5 de ASP.NET, consulte [creación de una aplicación de ASP.NET MVC 5 con el inicio de sesión de Facebook y Google OAuth2 y OpenID](../security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md).
> 
> Habilitar estas credenciales en los sitios web proporciona una ventaja significativa porque millones de usuarios ya tienen cuentas con estos proveedores externos. Estos usuarios pueden estar más inclinados para registrarse en su sitio si no tienen que crear y recordar un nuevo conjunto de credenciales. Además, una vez que un usuario ha iniciado sesión a través de uno de estos proveedores, puede incorporar operaciones sociales desde el proveedor.

## <a name="what-youll-build"></a>Lo que va a compilar

Hay dos objetivos principales en este tutorial:

1. Permite que un usuario inicie sesión con las credenciales de un proveedor de OAuth.
2. Recupere la información de la cuenta del proveedor e integre esa información con el registro de la cuenta de su sitio.

Aunque los ejemplos de este tutorial se centran en el uso de Facebook como proveedor de autenticación, puede modificar el código para que use cualquiera de los proveedores. Los pasos para implementar cualquier proveedor son muy similares a los pasos que se muestran en este tutorial. Solo observará diferencias significativas al realizar llamadas directas al conjunto de API del proveedor.

## <a name="prerequisites"></a>Requisitos previos

- [Microsoft Visual Studio 2012](https://www.microsoft.com/visualstudio/eng/downloads#vs) o [Microsoft Visual Studio Express 2012 para web](https://www.microsoft.com/visualstudio/eng/downloads#d-2012-express)

O bien

- Microsoft Visual Studio 2010 SP1 o [Visual Web Developer Express 2010 SP1](https://www.microsoft.com/visualstudio/eng/downloads#d-2010-express)
- [ASP.NET MVC 4](https://go.microsoft.com/fwlink/?LinkId=243392)

Además, en este tema se da por supuesto que tiene conocimientos básicos sobre ASP.NET MVC y Visual Studio. Si necesita una introducción a ASP.NET MVC 4, consulte la introducción [a ASP.NET MVC 4](getting-started-with-aspnet-mvc4/intro-to-aspnet-mvc-4.md).

## <a name="create-the-project"></a>Crear el proyecto

En Visual Studio, cree una nueva aplicación Web de ASP.NET MVC 4 y asígnele el nombre &quot;OAuthMVC&quot;. Puede tener como destino .NET Framework 4,5 o 4.

![creación de proyecto](using-oauth-providers-with-mvc/_static/image1.png)

En la ventana nuevo proyecto de ASP.NET MVC 4, seleccione **aplicación de Internet** y deje **Razor** como motor de vista.

![Seleccionar aplicación de Internet](using-oauth-providers-with-mvc/_static/image2.png)

## <a name="enable-a-provider"></a>Habilitación de un proveedor

Cuando se crea una aplicación Web de MVC 4 con la plantilla de aplicación de Internet, el proyecto se crea con un archivo denominado AuthConfig.cs en la carpeta de inicio de la aplicación\_.

![Archivo AuthConfig](using-oauth-providers-with-mvc/_static/image3.png)

El archivo AuthConfig contiene código para registrar clientes para proveedores de autenticación externos. De forma predeterminada, este código está marcado como comentario, por lo que no se habilita ninguno de los proveedores externos.

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample1.cs)]

Debe quitar la marca de comentario de este código para usar el cliente de autenticación externo. Quite los comentarios de los proveedores que desea incluir en el sitio. En este tutorial, solo habilitará las credenciales de Facebook.

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample2.cs)]

Observe en el ejemplo anterior que el método incluye cadenas vacías para los parámetros de registro. Si intenta ejecutar la aplicación ahora, la aplicación produce una excepción de argumento porque no se permiten cadenas vacías para los parámetros. Para proporcionar valores válidos, debe registrar el sitio web con los proveedores externos, como se muestra en la sección siguiente.

## <a name="registering-with-an-external-provider"></a>Registro con un proveedor externo

Para autenticar a los usuarios con credenciales de un proveedor externo, debe registrar el sitio web con el proveedor. Al registrar el sitio, recibirá los parámetros (como clave o identificador y secreto) que se incluirán al registrar el cliente. Debe tener una cuenta con los proveedores que desee usar.

En este tutorial no se muestran todos los pasos que debe realizar para registrarse con estos proveedores. Normalmente, los pasos no son difíciles. Para registrar correctamente el sitio, siga las instrucciones que se proporcionan en dichos sitios. Para empezar a registrar el sitio, consulte el sitio para desarrolladores de:

- [Facebook](https://developers.facebook.com/)
- [Google](https://developers.google.com/)
- [Microsoft](http://manage.dev.live.com/)
- [Twitter](https://dev.twitter.com/)

Al registrar el sitio en Facebook, puede proporcionar &quot;&quot; localhost para el dominio del sitio y `&quot; http://localhost/&quot;` de la dirección URL, tal como se muestra en la imagen siguiente. El uso de localhost funciona con la mayoría de los proveedores, pero actualmente no funciona con el proveedor de Microsoft. Para el proveedor de Microsoft, debe incluir una dirección URL de sitio web válida.

![registrar sitio](using-oauth-providers-with-mvc/_static/image4.png)

En la imagen anterior, se han quitado los valores para el identificador de aplicación, el secreto de la aplicación y el correo electrónico de contacto. Cuando registre realmente su sitio, esos valores estarán presentes. Querrá anotar los valores de ID. de aplicación y secreto de la aplicación, ya que los agregará a la aplicación.

## <a name="creating-test-users"></a>Crear usuarios de prueba

Si no le importa usar una cuenta de Facebook existente para probar su sitio, puede omitir esta sección.

Puede crear fácilmente usuarios de prueba para su aplicación en la página de administración de aplicaciones de Facebook. Puede usar estas cuentas de prueba para iniciar sesión en su sitio. Para crear usuarios de prueba, haga clic en el vínculo **roles** en el panel de navegación izquierdo y haga clic en el vínculo **crear** .

![crear usuarios de prueba](using-oauth-providers-with-mvc/_static/image5.png)

El sitio de Facebook crea automáticamente el número de cuentas de prueba que solicita.

## <a name="adding-application-id-and-secret-from-the-provider"></a>Agregar el identificador y el secreto de la aplicación desde el proveedor

Ahora que ha recibido el identificador y el secreto de Facebook, vuelva al archivo AuthConfig y agréguelos como valores de parámetro. Los valores que se muestran a continuación no son valores reales.

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample3.cs)]

## <a name="log-in-with-external-credentials"></a>Inicio de sesión con credenciales externas

Eso es todo lo que tiene que hacer para habilitar las credenciales externas en el sitio. Ejecute la aplicación y haga clic en el vínculo de inicio de sesión situado en la esquina superior derecha. La plantilla reconoce automáticamente que ha registrado Facebook como proveedor e incluye un botón para el proveedor. Si registra varios proveedores, se incluye automáticamente un botón para cada uno.

![Inicio de sesión externo](using-oauth-providers-with-mvc/_static/image6.png)

En este tutorial no se explica cómo personalizar los botones de inicio de sesión para los proveedores externos. Para obtener esa información, consulte [Personalización de la interfaz de usuario de inicio de sesión al usar OAuth/OpenID](https://blogs.msdn.com/b/pranav_rastogi/archive/2012/08/24/customizing-the-login-ui-when-using-oauth-openid.aspx).

Haga clic en el botón de Facebook para iniciar sesión con las credenciales de Facebook. Al seleccionar uno de los proveedores externos, se le redirigirá a ese sitio y el servicio lo solicitará para iniciar sesión.

En la imagen siguiente se muestra la pantalla de inicio de sesión de Facebook. También se indica que está usando su cuenta de Facebook para iniciar sesión en un sitio denominado oauthmvcexample.

![autenticación de Facebook](using-oauth-providers-with-mvc/_static/image7.png)

Después de iniciar sesión con las credenciales de Facebook, una página informa al usuario de que el sitio tendrá acceso a información básica.

![solicitar permiso](using-oauth-providers-with-mvc/_static/image8.png)

Después de seleccionar **ir a aplicación**, el usuario debe registrarse en el sitio. La siguiente imagen muestra la página de registro después de que un usuario haya iniciado sesión con las credenciales de Facebook. Normalmente, el nombre de usuario se rellena previamente con un nombre del proveedor.

![register](using-oauth-providers-with-mvc/_static/image9.png)

Haga clic en registrar para completar **el** registro. Cierre el explorador.

Puede ver que la nueva cuenta se ha agregado a la base de datos. En Explorador de servidores, abra la base de datos **DefaultConnection** y abra la carpeta **tablas** .

![tablas de bases de datos](using-oauth-providers-with-mvc/_static/image10.png)

Haga clic con el botón secundario en la tabla **userprofile** y seleccione **Mostrar datos de tabla**.

![Mostrar datos](using-oauth-providers-with-mvc/_static/image11.png)

Verá la nueva cuenta que ha agregado. Examine los datos de la **\_** de la página de OAuthMembership. Verá más datos relacionados con el proveedor externo de la cuenta que acaba de agregar.

Si solo desea habilitar la autenticación externa, ha terminado. Sin embargo, puede integrar más información del proveedor en el nuevo proceso de registro del usuario, tal como se muestra en las secciones siguientes.

## <a name="create-models-for-additional-user-information"></a>Crear modelos para información de usuario adicional

Como ha observado en las secciones anteriores, no es necesario recuperar información adicional para que el registro de la cuenta integrada funcione. Sin embargo, la mayoría de los proveedores externos devuelven información adicional sobre el usuario. En las secciones siguientes se muestra cómo conservar esa información y guardarla en una base de datos. En concreto, retendrá los valores del nombre completo del usuario, el URI de la página web personal del usuario y un valor que indique si Facebook ha comprobado la cuenta.

Usará [migraciones de Code First](https://msdn.microsoft.com/data/jj591621) para agregar una tabla para almacenar información de usuario adicional. Está agregando la tabla a una base de datos existente, por lo que primero deberá crear una instantánea de la base de datos actual. Al crear una instantánea de la base de datos actual, puede crear posteriormente una migración que solo contenga la nueva tabla. Para crear una instantánea de la base de datos actual:

1. Abrir la **consola del administrador de paquetes**
2. Ejecute el comando **enable-Migrations** .
3. Ejecute el comando **Add-Migration Initial – IgnoreChanges**
4. Ejecutar el comando **Update-Database**

Ahora, agregará las nuevas propiedades. En la carpeta models, abra el archivo AccountModels.cs y busque la clase RegisterExternalLoginModel. La clase RegisterExternalLoginModel contiene valores que proceden del proveedor de autenticación. Agregue propiedades denominadas FullName y Link, como se indica a continuación.

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample4.cs?highlight=9-13)]

También en AccountModels.cs, agregue una nueva clase denominada ExtraUserInformation. Esta clase representa la nueva tabla que se creará en la base de datos.

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample5.cs)]

En la clase UsersContext, agregue el código resaltado a continuación para crear una propiedad DbSet para la nueva clase.

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample6.cs?highlight=9)]

Ahora está listo para crear la nueva tabla. Vuelva a abrir la consola del administrador de paquetes y esta vez:

1. Ejecute el comando **Add-Migration AddExtraUserInformation**
2. Ejecutar el comando **Update-Database**

La nueva tabla ya existe en la base de datos.

## <a name="retrieve-the-additional-data"></a>Recuperación de los datos adicionales

Hay dos maneras de recuperar datos de usuario adicionales. La primera manera es conservar los datos de usuario que se devuelven, de forma predeterminada, durante la solicitud de autenticación. La segunda manera es llamar específicamente a la API del proveedor y solicitar más información. Facebook devuelve automáticamente los valores de FullName y Link. Un valor que indica si Facebook ha comprobado que la cuenta se ha recuperado a través de una llamada a la API de Facebook. En primer lugar, rellenará los valores de FullName y Link y, posteriormente, obtendrá el valor comprobado.

Para recuperar los datos de usuario adicionales, abra el archivo **AccountController.CS** en la carpeta **Controllers** .

Este archivo contiene la lógica para el registro, el registro y la administración de cuentas. En concreto, observe los métodos denominados **ExternalLoginCallback** y **ExternalLoginConfirmation**. Dentro de estos métodos, puede agregar código para personalizar las operaciones de inicio de sesión externo de la aplicación. La primera línea del método **ExternalLoginCallback** contiene:

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample7.cs)]

Los datos de usuario adicionales se devuelven en la propiedad **ExtraData** del objeto **AuthenticationResult** que se devuelve desde el método **VerifyAuthentication** . El cliente de Facebook contiene los siguientes valores en la propiedad **ExtraData** :

- id
- name
- link
- gender
- AccessToken

Otros proveedores tendrán datos similares pero ligeramente diferentes en la propiedad ExtraData.

Si el usuario es nuevo en su sitio, recuperará algunos datos adicionales y pasará los datos a la vista de confirmación. El último bloque de código del método solo se ejecuta si el usuario es nuevo en su sitio. Reemplace la línea siguiente:

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample8.cs)]

por esta otra:

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample9.cs)]

Este cambio solo incluye los valores de las propiedades FullName y Link.

En el método **ExternalLoginConfirmation** , modifique el código como se resalta a continuación para guardar la información de usuario adicional.

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample10.cs?highlight=4,7-13)]

## <a name="adjusting-the-view"></a>Ajustar la vista

Los datos de usuario adicionales que recupere del proveedor se mostrarán en la página de registro.

En la carpeta **views**/**account** , Abra **ExternalLoginConfirmation. cshtml**. Debajo del campo existente para nombre de usuario, agregue campos para FullName, Link y PictureLink.

[!code-cshtml[Main](using-oauth-providers-with-mvc/samples/sample11.cshtml)]

Ya está casi listo para ejecutar la aplicación y registrar un nuevo usuario con la información adicional guardada. Debe tener una cuenta que aún no esté registrada en el sitio. Puede usar una cuenta de prueba diferente o eliminar las filas de **userprofile** y **páginas web\_tablas OAuthMembership** para la cuenta que desea reutilizar. Al eliminar esas filas, se asegurará de que la cuenta se vuelva a registrar.

Ejecute la aplicación y registre el nuevo usuario. Tenga en cuenta que esta vez la página de confirmación contiene más valores.

![register](using-oauth-providers-with-mvc/_static/image12.png)

Después de completar el registro, cierre el explorador. Mire en la base de datos para ver los nuevos valores de la tabla **ExtraUserInformation** .

## <a name="install-nuget-package-for-facebook-api"></a>Instalación del paquete NuGet para la API de Facebook

Facebook proporciona una [API](https://developers.facebook.com/docs/reference/apis/) que se puede llamar para realizar operaciones. Puede llamar a la API de Facebook mediante la dirección de envío de solicitudes HTTP o mediante la instalación de un paquete NuGet que facilite el envío de esas solicitudes. En este tutorial se muestra el uso de un paquete NuGet, pero no es esencial instalar el paquete NuGet. En este tutorial se muestra cómo usar el C# paquete del SDK de Facebook. Hay otros paquetes de NuGet que ayudan a llamar a la API de Facebook.

En la ventana **administrar paquetes NuGet** , seleccione el paquete C# del SDK de Facebook.

![instalar paquete](using-oauth-providers-with-mvc/_static/image13.png)

Usará el SDK de Facebook C# para llamar a una operación que requiera el token de acceso para el usuario. En la sección siguiente se muestra cómo obtener el token de acceso.

## <a name="retrieve-access-token"></a>Recuperar el token de acceso

La mayoría de los proveedores externos pasan un token de acceso una vez comprobadas las credenciales del usuario. Este token de acceso es muy importante porque le permite llamar a las operaciones que solo están disponibles para los usuarios autenticados. Por lo tanto, es esencial recuperar y almacenar el token de acceso cuando se desea proporcionar más funcionalidad.

Dependiendo del proveedor externo, el token de acceso puede ser válido solo durante un período de tiempo limitado. Para asegurarse de que tiene un token de acceso válido, lo recuperará cada vez que el usuario inicie sesión y lo almacenará como un valor de sesión en lugar de guardarlo en una base de datos.

En el método **ExternalLoginCallback** , el token de acceso también se devuelve en la propiedad **ExtraData** del objeto **AuthenticationResult** . Agregue el código resaltado a **ExternalLoginCallback** para guardar el token de acceso en el objeto de **sesión** . Este código se ejecuta cada vez que el usuario inicia sesión con una cuenta de Facebook.

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample12.cs?highlight=11-14)]

Aunque en este ejemplo se recupera un token de acceso de Facebook, puede recuperar el token de acceso desde cualquier proveedor externo a través de la misma clave llamada &quot;AccessToken&quot;.

## <a name="logging-off"></a>Cerrar sesión

El método de **cierre de sesión** predeterminado cierra la sesión del usuario de la aplicación, pero no cierra la sesión del usuario del proveedor externo. Para cerrar la sesión del usuario en Facebook y evitar que el token se conserve una vez que el usuario haya cerrado la sesión, puede Agregar el siguiente código resaltado al método **LogOff** en AccountController.

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample13.cs?highlight=6-14)]

El valor que se proporciona en el parámetro `next` es la dirección URL que se utilizará después de que el usuario haya cerrado la sesión de Facebook. Al realizar pruebas en el equipo local, debe proporcionar el número de puerto correcto para el sitio local. En producción, se proporcionaría una página predeterminada, como contoso.com.

## <a name="retrieve-user-information-that-requires-the-access-token"></a>Recuperar la información de usuario que requiere el token de acceso

Ahora que ha almacenado el token de acceso y ha instalado el C# paquete del SDK de Facebook, puede usarlos juntos para solicitar información de usuario adicional de Facebook. En el método **ExternalLoginConfirmation** , cree una instancia de la clase **FacebookClient** pasando el valor del token de acceso. Solicite el valor de la propiedad **comprobado** para el usuario autenticado actual. La propiedad **comprobód** indica si Facebook ha validado la cuenta a través de otros medios, por ejemplo, para enviar un mensaje a un teléfono móvil. Guarde este valor en la base de datos.

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample14.cs?highlight=7-18,25)]

Tendrá que eliminar los registros de la base de datos para el usuario, o bien usar una cuenta de Facebook diferente.

Ejecute la aplicación y registre el nuevo usuario. Examine la tabla **ExtraUserInformation** para ver el valor de la propiedad comprobado.

## <a name="conclusion"></a>Conclusión

En este tutorial, ha creado un sitio que se integra con Facebook para la autenticación de usuario y los datos de registro. Aprendió el comportamiento predeterminado que se configura para la aplicación web MVC 4 y cómo personalizar ese comportamiento predeterminado.

## <a name="related-topics"></a>Temas relacionados

- [Crear una aplicación ASP.NET MVC con la autenticación y Base de datos SQL e implementar a Azure App Service](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data)
