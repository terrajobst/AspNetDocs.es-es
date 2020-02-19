---
uid: identity/overview/getting-started/developing-aspnet-apps-with-windows-azure-active-directory
title: Desarrollo de aplicaciones de ASP.NET con Azure Active Directory-ASP.NET 4. x
author: Rick-Anderson
description: Microsoft ASP.NET herramientas para Azure Active Directory facilita la habilitación de la autenticación para las aplicaciones web hospedadas en Azure. Puede usar Azure autenti...
ms.author: riande
ms.date: 08/14/2014
ms.assetid: 457d7eaf-ee76-4ceb-9082-c7c1721435ad
ms.custom: seoapril2019
msc.legacyurl: /identity/overview/getting-started/developing-aspnet-apps-with-windows-azure-active-directory
msc.type: authoredcontent
ms.openlocfilehash: 28425ea8d1312dfc6e14df9677396f2cbcf6f16d
ms.sourcegitcommit: 7709c0a091b8d55b7b33bad8849f7b66b23c3d72
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/19/2020
ms.locfileid: "77456730"
---
# <a name="developing-aspnet-apps-with-azure-active-directory"></a>Desarrollo de aplicaciones ASP.NET con Azure Active Directory

por [Rick Anderson](https://twitter.com/RickAndMSFT)

Las herramientas de Microsoft ASP.NET para Azure Active Directory simplifican la habilitación de la autenticación para las aplicaciones web hospedadas en [Azure](https://www.windowsazure.com/home/features/web-sites/). Puede usar la autenticación de Azure para autenticar a usuarios de Office 365 de su organización, cuentas corporativas sincronizadas desde su Active Directory local o usuarios creados en su propio dominio de Azure Active Directory personalizado. Al habilitar la autenticación de Windows Azure, se configura la aplicación para autenticar a los usuarios mediante un solo inquilino de [Azure Active Directory](https://docs.microsoft.com/azure/active-directory/) .

En este tutorial se muestra cómo crear una aplicación ASP.NET que está configurada para el inicio de sesión con [Azure Active Directory](https://msdn.microsoft.com/library/azure/mt168838.aspx) (Azure ad). También aprenderá a llamar a la Graph API para obtener información sobre el usuario que ha iniciado sesión actualmente y cómo implementar la aplicación en Azure.

## <a name="prerequisites"></a>Prerequisites

1. [Visual Studio Express 2013 para web](https://my.visualstudio.com/Downloads?q=visual%20studio%202013#d-2013-express) o [Visual Studio 2013](https://my.visualstudio.com/Downloads?q=visual%20studio%202013).
2. Se requiere [Visual Studio 2013 update 4](https://www.microsoft.com/download/details.aspx?id=44921) -Update 3 o posterior.
3. Una cuenta de Azure. [Haga clic aquí](https://azure.microsoft.com/pricing/free-trial/) para obtener una evaluación gratuita si aún no tiene una cuenta.

## <a name="add-a-global-administrator-to-your-active-directory"></a>Agregue un administrador global a su Active Directory

1. Inicie sesión en el [Portal de administración de Azure](https://manage.windowsazure.com/).
2. Todas las cuentas de Azure contienen un **directorio predeterminado** , haga clic en ella y, a continuación, haga clic en la pestaña **usuarios** en la parte superior de la página (vea la imagen siguiente).
3. Haga clic en Agregar usuario.
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image1.png)
4. Cree un nuevo usuario con el rol de **administrador global** . Haga clic en **usuarios** en el menú superior y, a continuación, haga clic en el botón **Agregar usuario** de la barra de comandos.
5. En el cuadro de diálogo **Agregar usuario** , escriba un nombre para el nuevo usuario y, a continuación, haga clic en la flecha derecha.

    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image2.png)
6. Escriba el nombre de usuario y establezca el rol en **administrador global**. Los administradores globales requieren una dirección de correo electrónico alternativa para la recuperación de contraseñas. Cuando haya terminado, haga clic en la flecha derecha.

    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image3.png)
7. En la página siguiente del cuadro de diálogo, haga clic en **crear**. Se creará una contraseña temporal para el nuevo usuario y se mostrará en el cuadro de diálogo.

    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image4.png)

   Guardar la contraseña, se le pedirá que cambie la contraseña después del primer inicio de sesión. En la imagen siguiente se muestra la nueva cuenta de administrador. Debe usar la Azure Active Directory para iniciar sesión en la aplicación, no en la cuenta de Microsoft también se muestra en esta página.

    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image5.png)

## <a name="create-an-aspnet-application"></a>Creación de una aplicación de ASP.NET

En los pasos siguientes se usa [Visual Studio Express 2013 para web](https://www.microsoft.com/download/details.aspx?id=40747)y se requiere [Visual Studio 2013 Update 3](https://www.microsoft.com/download/details.aspx?id=43721).

1. En Visual Studio, haga clic en **archivo** y, a continuación, en **nuevo proyecto**. En el cuadro de diálogo **nuevo proyecto** , seleccione C# el proyecto web visual en el menú de la izquierda y haga clic en **Aceptar**. También puede desactivar **agregar Application Insights a proyecto** si no desea la funcionalidad de la aplicación.
2. En el cuadro de diálogo **nuevo proyecto ASP.net** , seleccione **MVC**y, a continuación, haga clic en **cambiar autenticación**.

    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image6.png)
3. En el cuadro de diálogo **cambiar autenticación** , seleccione **cuentas organizativas**. Estas opciones se pueden usar para registrar automáticamente la aplicación con Azure AD y configurar automáticamente la aplicación para que se integre con Azure AD. No tiene que usar el cuadro de diálogo **cambiar autenticación** para registrar y configurar la aplicación, pero hace que sea mucho más fácil. Si usa Visual Studio 2012 por ejemplo, puede registrar manualmente la aplicación en Azure Portal de administración y actualizar su configuración para que se integre con Azure AD.
   En los menús desplegables, seleccione **la nube de una sola organización** y el **Inicio de sesión único, leer datos de directorio**. Escriba el dominio de su Azure AD directorio, por ejemplo (en las imágenes siguientes) *aricka0yahoo.onmicrosoft.com*y, a continuación, haga clic en **Aceptar**. Puede obtener el nombre de dominio en la pestaña dominios del directorio predeterminado en el portal de Azure (consulte la imagen siguiente).

    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image7.png)

   En la imagen siguiente se muestra el nombre de dominio del Azure Portal.

    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image8.png)

    > [!NOTE]
    > Opcionalmente, puede configurar el URI del ID. de aplicación que se registrará en Azure AD haciendo clic en **más opciones**. El URI de ID. de aplicación es el identificador único de una aplicación, que se registra en Azure AD y lo usa la aplicación para identificarse al comunicarse con Azure AD. Para obtener más información sobre el URI de ID. de aplicación y otras propiedades de aplicaciones registradas, consulte [este tema](https://msdn.microsoft.com/library/azure/dn499820.aspx#BKMK_Registering). Al hacer clic en la casilla situada debajo del campo URI de ID. de aplicación, también puede optar por sobrescribir un registro existente en Azure AD que usa el mismo URI de ID. de aplicación.
4. Después de hacer clic en **Aceptar**, aparecerá un cuadro de diálogo de inicio de sesión que tendrá que iniciar sesión con una cuenta de administrador global (no la cuenta de Microsoft asociada a su suscripción). Si ha creado una cuenta de administrador anterior, deberá cambiar la contraseña y, a continuación, volver a iniciar sesión con la nueva contraseña.

    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image9.png)
5. Una vez que se haya autenticado correctamente, el cuadro de diálogo **nuevo proyecto de ASP.net** mostrará la opción de autenticación (**organización** ) y el directorio donde se registrará la nueva aplicación (*aricka0yahoo.onmicrosoft.com* en la siguiente imagen). Debajo de esta información, active la casilla **host en la nube**. Si esta casilla está activada, el proyecto se aprovisionará como una aplicación Web de Azure y se habilitará para facilitar su publicación más adelante. Haga clic en **OK**.

    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image10.png)
6. Aparecerá el cuadro de diálogo **configurar sitio web de Azure** con una región y un nombre de sitio generados automáticamente. Tenga en cuenta también la cuenta en la que ha iniciado sesión actualmente en el cuadro de diálogo. Desea asegurarse de que esta cuenta es la a la que está asociada la suscripción de Azure, normalmente una cuenta de Microsoft.

    > [!NOTE]
    > Este proyecto requiere una base de datos. Debe seleccionar una de las bases de datos existentes o crear una nueva. Se necesita una base de datos porque el proyecto ya usa un archivo de base de datos local para almacenar una pequeña cantidad de datos de configuración de autenticación. Al implementar la aplicación en un sitio web de Azure, esta base de datos no está empaquetada con la implementación, por lo que debe elegir una que sea accesible en la nube. Haga clic en **OK**.

    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image11.png)
7. Se creará el proyecto y las opciones de autenticación y las opciones de la aplicación web se configurarán automáticamente con el proyecto. Una vez completado este proceso, ejecute el proyecto de forma local presionando **^ F5**. Se le pedirá que inicie sesión con su cuenta de organización. Proporcione el nombre de usuario y la contraseña de la cuenta que creó anteriormente y haga clic en **iniciar sesión**.

    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image12.png)
8. Después de iniciar sesión correctamente, el sitio de ASP.NET mostrará que se ha autenticado mostrando el nombre de usuario en la esquina superior derecha de la página.

    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image13.png)

   Si obtiene el error: el valor no puede ser null ni estar vacío. Nombre del parámetro: linkText ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image14.png)

   Vea la sección [depurar](#dbg) al final del tutorial.

## <a name="basics-of-the-graph-api"></a>Aspectos básicos del Graph API

[El Graph API](https://msdn.microsoft.com/library/azure/hh974476.aspx) es la interfaz de programación que se usa para realizar las operaciones CRUD y otras en los objetos del directorio Azure ad. Si selecciona una opción de cuenta de la organización para la autenticación al crear un nuevo proyecto en Visual Studio 2013, la aplicación ya estará configurada para llamar a la Graph API. En esta sección se muestra brevemente cómo funciona el Graph API.

1. En la aplicación en ejecución, haga clic en el nombre del usuario con sesión iniciada en la parte superior derecha de la página. Esto le llevará a la página de Perfil de usuario, que es una acción en el controlador Home. Observará que la tabla contiene información de usuario acerca de la cuenta de administrador que creó anteriormente. Esta información se almacena en el directorio y se llama al Graph API para recuperar esta información cuando se carga la página.

    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image15.png)
2. Vuelva a Visual Studio y expanda la carpeta **Controllers** y, a continuación, abra el archivo **HomeController.CS** . Verá una acción **userprofile ()** que contiene el código para recuperar un token y, a continuación, llamar a la Graph API. Este código se duplica a continuación:

    [!code-csharp[Main](developing-aspnet-apps-with-windows-azure-active-directory/samples/sample1.cs?highlight=22)]

    Para llamar al Graph API, primero debe recuperar un token. Cuando se recupera el token, su valor de cadena debe anexarse en el encabezado de autorización para todas las solicitudes posteriores a la Graph API. La mayoría del código anterior controla los detalles de la autenticación en Azure AD para obtener un token, el uso del token para realizar una llamada al Graph API y, a continuación, la transformación de la respuesta para que se pueda presentar en la vista.

    La parte más relevante de la discusión es la siguiente línea resaltada: `UserProfile profile = JsonConvert.DeserializeObject<UserProfile>(responseString);`. Esta línea representa el nombre del usuario, que se ha deserializado de la respuesta JSON y se presenta en la vista.

    Puede llamar al Graph API mediante HttpClient y administrar los datos sin procesar usted mismo, pero una forma más sencilla es usar la [biblioteca de cliente de gráficos que está disponible a través de NuGet](http://www.nuget.org/packages/Microsoft.Azure.ActiveDirectory.GraphClient/). La biblioteca de cliente controla las solicitudes HTTP sin formato y la transformación de los datos devueltos, y facilita el trabajo con el Graph API en un entorno de .NET. Vea los ejemplos de código Graph API relacionados en [github.](https://github.com/AzureADSamples)

## <a name="deploy-the-application-to-azure"></a>Implementación de la aplicación en Azure

En los pasos siguientes se muestra cómo implementar la aplicación en Azure. En los pasos anteriores, conectó el nuevo proyecto con una aplicación web en Azure, por lo que está listo para publicarse en unos pocos pasos.

1. En Visual Studio, haga clic con el botón derecho en el proyecto y seleccione **publicar**. Aparecerá el cuadro de diálogo **publicar web** con cada configuración ya configurada. Haga clic en el botón **siguiente** para ir a la página **configuración** . Es posible que se le pida que se autentique; Asegúrese de autenticarse con su cuenta de suscripción de Azure (normalmente una cuenta de Microsoft) y no la cuenta de la organización que creó anteriormente.

    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image16.png)
2. Active la opción **Habilitar autenticación organizativa** . En el campo **dominio** , escriba el dominio del directorio. En el menú desplegable **nivel de acceso** , seleccione **Inicio de sesión único, leer datos de directorio**. Observará que la base de datos anterior que usó ya se ha rellenado en la sección **bases de datos** . Haga clic en **Publicar**.

    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image17.png)
3. Visual Studio comenzará a implementar el sitio web y, a continuación, aparecerá una nueva ventana del explorador. Es posible que se le pida que se autentique de nuevo en el directorio. Una vez que se haya autenticado, se le redirigirá al sitio web recién publicado en Azure.

    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image18.png)

<a id="dbg"></a>
## <a name="debugging-the-app"></a>Depurar la aplicación

Si recibe el siguiente error: el valor no puede ser null ni estar vacío. Nombre del parámetro: linkText

![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image19.png)

Reemplace el código del archivo *Views\Shared\\_LoginPartial. cshtml* con lo siguiente:

[!code-cshtml[Main](developing-aspnet-apps-with-windows-azure-active-directory/samples/sample2.cshtml?highlight=1-8,15-16)]

Después de ejecutar la aplicación, si el usuario que ha iniciado sesión muestra "usuario nulo", cierre sesión y vuelva a iniciar sesión con la cuenta de Active Directory que creó anteriormente.

Un tutorial excelente que debe seguir es el análisis detallado de Rick Rainey [: Azure websites y la autenticación de la organización con Azure ad](http://rickrainey.com/2014/08/19/deep-dive-azure-websites-and-organizational-authentication-using-azure-ad/).

## <a name="more-information"></a>Más información

- [Profundización: Azure websites y autenticación de la organización con Azure AD](http://rickrainey.com/2014/08/19/deep-dive-azure-websites-and-organizational-authentication-using-azure-ad/)
- [Información general sobre Graph API de Azure AD](https://msdn.microsoft.com/library/azure/hh974476.aspx)
- [Escenarios de autenticación en Azure AD](https://msdn.microsoft.com/library/azure/dn499820.aspx)
- [Azure AD ejemplos de código en GitHub](https://github.com/AzureADSamples)
