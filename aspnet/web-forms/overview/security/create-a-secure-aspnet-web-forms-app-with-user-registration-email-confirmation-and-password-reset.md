---
uid: web-forms/overview/security/create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset
title: Creación de una aplicación de formularios Web Forms de ASP.NET segura con registro de usuarios,C#confirmación de correo electrónico y restablecimiento de contraseña () | Microsoft Docs
author: Erikre
description: En este tutorial se muestra cómo compilar una aplicación de formularios Web Forms ASP.NET con registro de usuarios, confirmación de correo electrónico y restablecimiento de contraseña mediante el miembro de ASP.NET Identity...
ms.author: riande
ms.date: 10/02/2014
ms.assetid: 0a8d6044-5fab-4213-82d6-5618d5601358
msc.legacyurl: /web-forms/overview/security/create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset
msc.type: authoredcontent
ms.openlocfilehash: af3653bc164810126bc3bf8f1b1794d75642d807
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78507133"
---
# <a name="create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset-c"></a>Crear una aplicación segura de formularios Web Forms de ASP.NET con registro de usuario, confirmación por correo electrónico y restablecimiento de contraseña (C#)

por [Erik Reitan](https://github.com/Erikre)

> En este tutorial se muestra cómo compilar una aplicación de formularios Web Forms ASP.NET con registro de usuarios, confirmación de correo electrónico y restablecimiento de contraseña mediante el sistema de pertenencia de ASP.NET Identity. Este tutorial se basó en el tutorial de [MVC](../../../mvc/overview/security/create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset.md)de Rick Anderson.

## <a name="introduction"></a>Introducción

Este tutorial le guía a través de los pasos necesarios para crear una aplicación de formularios Web Forms de ASP.NET mediante Visual Studio y ASP.NET 4,5 para crear una aplicación de formularios Web Forms segura con registro de usuarios, confirmación de correo electrónico y restablecimiento de contraseña.

### <a name="tutorial-tasks-and-information"></a>Tareas e información del tutorial:

- [Creación de una aplicación de formularios Web Forms ASP.NET](#createWebForms)
- [Enlazar SendGrid](#SG)
- [Requerir confirmación por correo electrónico antes de iniciar sesión](#require)
- [Recuperación y restablecimiento de contraseñas](#reset)
- [Vínculo reenviar correo electrónico de confirmación](#rsend)
- [Solución de problemas de la aplicación](#dbg)
- [Recursos adicionales](#addRes)

<a id="createWebForms"></a>
## <a name="create-an-aspnet-web-forms-app"></a>Creación de una aplicación de formularios Web Forms ASP.NET

Empiece por instalar y ejecutar [Visual Studio Express 2013 para web](https://go.microsoft.com/fwlink/?LinkId=299058) o [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566). Instale también [Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465) o una versión posterior.

> [!NOTE]
> ADVERTENCIA: debe instalar [Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465) o superior para completar este tutorial.

1. Cree un nuevo proyecto (**archivo** -&gt; **nuevo proyecto**) y seleccione la plantilla **aplicación Web ASP.net** y la versión más reciente de la .NET Framework en el cuadro de diálogo **nuevo proyecto** .
2. En el cuadro de diálogo **nuevo proyecto de ASP.net** , seleccione la plantilla de **formularios Web Forms** . Deje la autenticación predeterminada como **cuentas de usuario individuales**. Si desea hospedar la aplicación en Azure, deje activada la casilla **hospedar en la nube** .   
 A continuación, haga clic en **Aceptar** para crear el nuevo proyecto.  
    ![Cuadro de diálogo New ASP.NET Project](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/_static/image1.png)
3. Habilite Capa de sockets seguros (SSL) para el proyecto. Siga los pasos disponibles en la sección **habilitación de SSL para el proyecto** de la [serie de tutoriales de introducción con formularios Web Forms](../getting-started/getting-started-with-aspnet-45-web-forms/checkout-and-payment-with-paypal.md#SSLWebForms).
4. Ejecute la aplicación, haga clic en el vínculo **registrar** y registre un nuevo usuario. En este momento, la única validación en el correo electrónico se basa en el atributo [[EmailAddress]](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.emailaddressattribute(v=vs.110).aspx) para asegurarse de que la dirección de correo electrónico tiene el formato correcto. Modificará el código para agregar una confirmación por correo electrónico. Cierre la ventana del explorador.
5. En **Explorador de servidores** de Visual Studio (**vista** -&gt; **Explorador de servidores**), vaya a **datos Connections\DefaultConnection\Tables\AspNetUsers**, haga clic con el botón derecho y seleccione **abrir definición de tabla**. 

    En la imagen siguiente se muestra el esquema de tabla `AspNetUsers`:

    ![Esquema de la tabla AspNetUsers](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/_static/image2.png)
6. En **Explorador de servidores**, haga clic con el botón derecho en la tabla **AspNetUsers** y seleccione **Mostrar datos de tabla**.  
  
    ![Datos de la tabla AspNetUsers](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/_static/image3.png)  
 En este momento no se ha confirmado el correo electrónico del usuario registrado.
7. Haga clic en la fila y seleccione eliminar para eliminar el usuario. Agregará este correo electrónico de nuevo en el paso siguiente y enviará un mensaje de confirmación a la dirección de correo electrónico.

## <a name="email-confirmation"></a>Confirmación por correo electrónico

Se recomienda confirmar el correo electrónico durante el registro de un nuevo usuario para comprobar que no está suplantando a otra persona (es decir, que no se han registrado con el correo electrónico de otra persona). Supongamos que tiene un foro de discusión, desea evitar que `"bob@cpandl.com"` se registre como `"joe@contoso.com"`. Sin confirmación por correo electrónico, `"joe@contoso.com"` podría obtener un correo electrónico no deseado de la aplicación. Supongamos que Bob se registra accidentalmente como `"bib@cpandl.com"` y no lo ha notado, no podría usar la recuperación de contraseña porque la aplicación no tiene su correo electrónico correcto. La confirmación por correo electrónico proporciona solo protección limitada de bots y no proporciona protección contra los remitentes de spam determinados.

Por lo general, querrá evitar que los nuevos usuarios publiquen datos en el sitio Web antes de que se hayan confirmado mediante correo electrónico, un mensaje de texto SMS u otro mecanismo. En las secciones siguientes, habilitaremos la confirmación por correo electrónico y modificaremos el código para evitar que los usuarios recién registrados inicien sesión hasta que se haya confirmado su correo electrónico. En este tutorial, usará el servicio de correo electrónico SendGrid.

<a id="SG"></a>
## <a name="hook-up-sendgrid"></a>Enlazar SendGrid

SendGrid ha cambiado su API desde que se escribió este tutorial. Para obtener instrucciones de SendGrid actuales, consulte [SendGrid](http://sendgrid.com/) o [Habilitar la confirmación de cuenta y la recuperación de contraseña](xref:security/authentication/accconfirm#enable-account-confirmation-and-password-recovery).

Aunque este tutorial solo muestra cómo agregar una notificación por correo electrónico a través de [SendGrid](http://sendgrid.com/), puede enviar correo electrónico mediante SMTP y otros mecanismos (consulte [recursos adicionales](#addRes)).

1. En Visual Studio, abra la **consola del administrador de paquetes** (**herramientas** -&gt; **administrador de paquetes NuGet** -&gt; **consola del administrador de paquetes**) y escriba el siguiente comando:  
    `Install-Package SendGrid`
2. Vaya a la página de registro de [Azure SendGrid](https://azure.microsoft.com/gallery/store/sendgrid/sendgrid-azure/) y regístrese para obtener una cuenta gratuita de SendGrid. También puede registrarse para obtener una cuenta gratuita de SendGrid directamente en el [sitio de SendGrid](http://www.sendgrid.com).
3. En **Explorador de soluciones** Abra el archivo *IdentityConfig.CS* en la carpeta de inicio de la\_de la *aplicación* y agregue el siguiente código resaltado en amarillo a la clase `EmailService` para configurar **SendGrid**:

    [!code-csharp[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample1.cs?highlight=3,5,8-37)]
4. Además, agregue las siguientes instrucciones de `using` al principio del archivo *IdentityConfig.CS* : 

    [!code-csharp[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample2.cs?highlight=1-4)]
5. Para simplificar este ejemplo, almacene los valores de la cuenta de servicio de correo electrónico en la sección `appSettings` del archivo *Web. config* . Agregue el siguiente código XML resaltado en amarillo al archivo *Web. config* en la raíz del proyecto:

    [!code-xml[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample3.xml?highlight=2-5)]

    > [!WARNING]
    > Seguridad: nunca almacene datos confidenciales en el código fuente. En este ejemplo, la cuenta y las credenciales se almacenan en la sección **appSetting** del archivo *Web. config* . En Azure, puede almacenar estos valores de forma segura en la pestaña **[configurar](https://blogs.msdn.com/b/webdev/archive/2014/06/04/queuebackgroundworkitem-to-reliably-schedule-and-run-long-background-process-in-asp-net.aspx)** del Azure portal. Para obtener información relacionada, vea el tema de Rick Anderson titulado [procedimientos recomendados para la implementación de contraseñas y otros datos confidenciales en ASP.net y Azure](https://go.microsoft.com/fwlink/?LinkId=513141).
6. Agregue los valores del servicio de correo electrónico para reflejar los valores de autenticación de SendGrid (nombre de usuario y contraseña) para poder enviar correo electrónico correctamente desde la aplicación. Asegúrese de usar el nombre de la cuenta de SendGrid en lugar de la dirección de correo electrónico que proporcionó SendGrid.

### <a name="enable-email-confirmation"></a>Habilitar confirmación de correo electrónico

 Para habilitar la confirmación por correo electrónico, modificará el código de registro siguiendo estos pasos.  

1. En la carpeta de la *cuenta* , abra el código subyacente de *Register.aspx.CS* y actualice el método `CreateUser_Click` para habilitar los siguientes cambios resaltados: 

    [!code-csharp[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample4.cs?highlight=9-11)]
2. En **Explorador de soluciones**, haga clic con el botón secundario en *default. aspx* y seleccione **establecer como página de inicio**.
3. Ejecute la aplicación presionando **F5.** Una vez que se muestre la página, haga clic en el vínculo **registrar** para mostrar la página de registro.
4. Escriba el correo electrónico y la contraseña y haga clic en el botón **registrar** para enviar un mensaje de correo electrónico a través de SendGrid.  
   El estado actual del proyecto y el código permitirá que el usuario inicie sesión una vez que complete el formulario de registro, aunque no haya confirmado su cuenta.
5. Compruebe su cuenta de correo electrónico y haga clic en el vínculo para confirmar su correo electrónico.  
   Una vez enviado el formulario de registro, se iniciará sesión.  
    ![Sitio web de ejemplo-sesión iniciada](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/_static/image4.png)

<a id="require"></a>
## <a name="require-email-confirmation-before-log-in"></a>Requerir confirmación por correo electrónico antes de iniciar sesión

Aunque haya confirmado la cuenta de correo electrónico, en este momento no tendría que hacer clic en el vínculo contenido en el correo electrónico de comprobación para que la sesión esté completa. En la siguiente sección, modificará el código que requiere que los nuevos usuarios tengan un correo electrónico confirmado antes de que se inicie sesión (autenticado).

1. En **Explorador de soluciones** de Visual Studio, actualice el evento de `CreateUser_Click` en el código subyacente de *Register.aspx.CS* incluido en la carpeta *cuentas* con los siguientes cambios resaltados: 

    [!code-csharp[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample5.cs?highlight=13-14,17-21)]
2. Actualice el método de `LogIn` en el código subyacente de *login.aspx.CS* con los siguientes cambios resaltados: 

    [!code-csharp[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample6.cs?highlight=9-19,45-46)]

### <a name="run-the-application"></a>Ejecutar la aplicación

 Ahora que ha implementado el código para comprobar si se ha confirmado la dirección de correo electrónico de un usuario, puede comprobar la funcionalidad en las páginas **registro** e **Inicio de sesión** . 

1. Elimine todas las cuentas de la tabla **AspNetUsers** que contengan el alias de correo electrónico que desea probar.
2. Ejecute la aplicación (**F5**) y compruebe que no se puede registrar como usuario hasta que se haya confirmado su dirección de correo electrónico.
3. Antes de confirmar la nueva cuenta a través del correo electrónico que se acaba de enviar, intente iniciar sesión con la nueva cuenta.  
 Verá que no puede iniciar sesión y que debe tener una cuenta de correo electrónico confirmada.
4. Una vez confirmada la dirección de correo electrónico, inicie sesión en la aplicación.

<a id="reset"></a>
## <a name="password-recovery-and-reset"></a>Recuperación y restablecimiento de contraseñas

1. En Visual Studio, quite los caracteres de comentario del método `Forgot` en el código subyacente de *Forgot.aspx.CS* incluido en la carpeta de la *cuenta* , para que el método aparezca de la manera siguiente: 

    [!code-csharp[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample7.cs?highlight=16-18)]
2. Abra la página *login. aspx* . Reemplace el marcado cerca del final de la sección **loginForm** , tal y como se resalta a continuación: 

    [!code-aspx[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample8.aspx?highlight=52-53)]
3. Abra el código subyacente de *login.aspx.CS* y quite la marca de comentario de la siguiente línea de código resaltada en amarillo desde el controlador de eventos `Page_Load`: 

    [!code-csharp[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample9.cs?highlight=5)]
4. Ejecute la aplicación presionando **F5.** Una vez que se muestre la página, haga clic en el vínculo **iniciar sesión** .
5. Haga clic en el vínculo **¿olvidó su contraseña?** para mostrar la página **contraseña olvidada** .
6. Escriba su dirección de correo electrónico y haga clic en el botón **Enviar** para enviar un correo electrónico a su dirección que le permitirá restablecer la contraseña.   
   Compruebe su cuenta de correo electrónico y haga clic en el vínculo para mostrar la página **Restablecer contraseña** .
7. En la página **Restablecer contraseña** , escriba el correo electrónico, la contraseña y la contraseña confirmada. A continuación, haga clic en el botón **restablecer** .  
   Cuando haya restablecido correctamente la contraseña, se mostrará la página **contraseña cambiada** . Ahora puede iniciar sesión con la nueva contraseña.

<a id="rsend"></a>
## <a name="resend-email-confirmation-link"></a>Vínculo reenviar correo electrónico de confirmación

Una vez que un usuario crea una nueva cuenta local, se le envía un vínculo de confirmación para que pueda iniciar sesión. Si el usuario elimina accidentalmente el correo electrónico de confirmación o el correo electrónico nunca llega, deberá volver a enviar el vínculo de confirmación. Los siguientes cambios de código muestran cómo habilitarlo.

1. En Visual Studio, abra el código subyacente de **login.aspx.CS** y agregue el siguiente controlador de eventos después del controlador de eventos `LogIn`:   

    [!code-csharp[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample10.cs)]
2. Modifique el controlador de eventos `LogIn` en el código subyacente de *login.aspx.CS* cambiando el código resaltado en amarillo de la manera siguiente: 

    [!code-csharp[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample11.cs?highlight=15-17)]
3. Actualice la página *login. aspx* agregando el código resaltado en amarillo de la siguiente manera: 

    [!code-aspx[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample12.aspx?highlight=45-46)]
4. Elimine todas las cuentas de la tabla **AspNetUsers** que contengan el alias de correo electrónico que desea probar.
5. Ejecute la aplicación (**F5**) y registre su dirección de correo electrónico.
6. Antes de confirmar la nueva cuenta a través del correo electrónico que se acaba de enviar, intente iniciar sesión con la nueva cuenta.  
   Verá que no puede iniciar sesión y que debe tener una cuenta de correo electrónico confirmada. Además, ahora puede volver a enviar un mensaje de confirmación a su cuenta de correo electrónico.
7. Escriba la dirección de correo electrónico y la contraseña y, después, presione el botón **volver a enviar confirmación** .
8. Una vez que confirme su dirección de correo electrónico en función del mensaje de correo electrónico recién enviado, inicie sesión en la aplicación.

<a id="dbg"></a>
## <a name="troubleshooting-the-app"></a>Solución de problemas de la aplicación

Si no recibe un correo electrónico que contenga el vínculo para comprobar sus credenciales:

- Compruebe la carpeta de correo no deseado o correo no deseado.
- Inicie sesión en su cuenta de SendGrid y haga clic en el [vínculo actividad de correo electrónico](https://sendgrid.com/logs/index).
- Asegúrese de usar el nombre de la cuenta de usuario de SendGrid como valor de *Web. config* en lugar de la dirección de correo electrónico de la cuenta de SendGrid.

<a id="addRes"></a>
## <a name="additional-resources"></a>Recursos adicionales

- [Vínculos a ASP.NET Identity recursos recomendados](../../../identity/overview/getting-started/aspnet-identity-recommended-resources.md)
- [Confirmación de la cuenta y recuperación de la contraseña con ASP.NET Identity](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md)
- [Serie de tutoriales de formularios Web Forms de ASP.NET: agregar un proveedor OAuth 2,0](../getting-started/getting-started-with-aspnet-45-web-forms/checkout-and-payment-with-paypal.md#OAuthWebForms)
- [Implementar una aplicación de formularios Web Forms de ASP.NET segura con pertenencia, OAuth y SQL Database para Azure App Service](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/)
- [Serie de tutoriales de ASP.NET Web Forms: habilitación de SSL para el proyecto](../getting-started/getting-started-with-aspnet-45-web-forms/checkout-and-payment-with-paypal.md#SSLWebForms)
