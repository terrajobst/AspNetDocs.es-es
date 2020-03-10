---
uid: web-pages/overview/getting-started/11-adding-email-to-your-web-site
title: Enviar correo electrónico desde un sitio de ASP.NET Web Pages (Razor) | Microsoft Docs
author: Rick-Anderson
description: En este capítulo se explica cómo enviar un mensaje de correo electrónico automatizado desde un sitio Web.
ms.author: riande
ms.date: 02/20/2014
ms.assetid: fc49bcb9-f1a9-4048-8c3f-b60951853200
msc.legacyurl: /web-pages/overview/getting-started/11-adding-email-to-your-web-site
msc.type: authoredcontent
ms.openlocfilehash: 23e9717329525fb5a0ed505c9dc94505d4f9dbbe
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78515329"
---
# <a name="sending-email-from-an-aspnet-web-pages-razor-site"></a>Enviar correo electrónico desde un sitio de ASP.NET Web Pages (Razor)

por [Tom FitzMacken](https://github.com/tfitzmac)

> En este artículo se explica cómo enviar un mensaje de correo electrónico desde un sitio web cuando se usa ASP.NET Web Pages (Razor).
> 
> Temas que se abordarán:
> 
> - Cómo enviar un mensaje de correo electrónico desde su sitio Web.
> - Cómo adjuntar un archivo a un mensaje de correo electrónico.
> 
> Esta es la característica ASP.NET presentada en el artículo:
> 
> - Aplicación auxiliar de `WebMail`.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versiones de software usadas en el tutorial
> 
> 
> - ASP.NET Web Pages (Razor) 3
>   
> 
> Este tutorial también funciona con ASP.NET Web Pages 2.

<a id="Sending_Email_Messages"></a>
## <a name="sending-email-messages-from-your-website"></a>Enviar mensajes de correo electrónico desde su sitio web

Hay todo tipo de razones por las que es posible que necesite enviar correo electrónico desde su sitio Web. Es posible que se envíen mensajes de confirmación a los usuarios o que se envíen notificaciones a sí mismo (por ejemplo, que se haya registrado un usuario nuevo). La aplicación auxiliar de `WebMail` facilita el envío de correo electrónico.

Para usar la aplicación auxiliar de `WebMail`, debe tener acceso a un servidor SMTP. (SMTP significa *Protocolo simple de transferencia de correo*). Un servidor SMTP es un servidor de correo electrónico que solo reenvía mensajes al servidor &#8212; del destinatario es el lado saliente del correo electrónico. Si usa un proveedor de hospedaje para su sitio web, es probable que lo configure con correo electrónico y que le indique cuál es el nombre del servidor SMTP. Si está trabajando dentro de una red corporativa, un administrador o el Departamento de TI normalmente puede proporcionarle la información sobre un servidor SMTP que puede usar. Si está trabajando en casa, es posible que incluso pueda realizar pruebas con su proveedor de correo electrónico ordinario, que puede indicarle el nombre de su servidor SMTP. Normalmente, necesita:

- Nombre del servidor SMTP.
- Número del puerto. Casi siempre es 25. Sin embargo, es posible que el ISP le solicite que use el puerto 587. Si utiliza capa de sockets seguros (SSL) para el correo electrónico, es posible que necesite un puerto diferente. Consulte con su proveedor de correo electrónico.
- Credenciales (nombre de usuario, contraseña).

En este procedimiento, creará dos páginas. La primera página tiene un formulario que permite a los usuarios escribir una descripción, como si estuvieran rellenando un formulario de soporte técnico. La primera página envía su información a una segunda página. En la segunda página, el código extrae la información del usuario y envía un mensaje de correo electrónico. También muestra un mensaje que confirma que se ha recibido el informe de problemas.

![[imagen]](11-adding-email-to-your-web-site/_static/image1.jpg)

> [!NOTE]
> Para simplificar este ejemplo, el código inicializa el `WebMail` auxiliar adecuado en la página donde se usa. Sin embargo, en el caso de los sitios Web reales, es una idea mejor colocar el código de inicialización como este en un archivo global, de modo que inicialice la aplicación auxiliar de `WebMail` para todos los archivos del sitio Web. Para obtener más información, vea [personalizar el comportamiento de todo el sitio para ASP.NET Web pages](https://go.microsoft.com/fwlink/?LinkId=202906#Setting_Values_For_Helpers).

1. Cree un nuevo sitio Web.
2. Agregue una nueva página denominada *EmailRequest. cshtml* y agregue el marcado siguiente: 

    [!code-html[Main](11-adding-email-to-your-web-site/samples/sample1.html)]

    Observe que el atributo `action` del elemento Form se ha establecido en *ProcessRequest. cshtml*. Esto significa que el formulario se enviará a la página en lugar de volver a la página actual.
3. Agregue una nueva página denominada *ProcessRequest. cshtml* al sitio web y agregue el siguiente código y marcado:   

    [!code-cshtml[Main](11-adding-email-to-your-web-site/samples/sample2.cshtml)]

    En el código, obtiene los valores de los campos de formulario que se enviaron a la página. A continuación, llame al método `Send` del ayudante `WebMail` para crear y enviar el mensaje de correo electrónico. En este caso, los valores que se van a usar se componen del texto que se concatena con los valores que se enviaron desde el formulario.

    El código de esta página se encuentra dentro de un bloque `try/catch`. Si por alguna razón el intento de enviar un correo electrónico no funciona (por ejemplo, la configuración no es la adecuada), se ejecuta el código del bloque `catch` y se establece la variable `errorMessage` en el error que se ha producido. (Para obtener más información sobre los bloques de `try/catch` o la etiqueta de `<text>`, consulte [Introduction to ASP.NET Web pages Programming Using the Razor Syntax](https://go.microsoft.com/fwlink/?LinkID=251587#ID_HandlingErrors)).

    En el cuerpo de la página, si la variable `errorMessage` está vacía (el valor predeterminado), el usuario verá un mensaje que indica que se ha enviado el mensaje de correo electrónico. Si la variable `errorMessage` está establecida en true, el usuario verá un mensaje que indica que se ha producido un problema al enviar el mensaje.

    Tenga en cuenta que en la parte de la página que muestra un mensaje de error, hay una prueba adicional: `if(debuggingFlag)`. Se trata de una variable que puede establecer en true si tiene problemas para enviar correo electrónico. Cuando `debuggingFlag` es true, y si hay un problema de envío de correo electrónico, se muestra un mensaje de error adicional que muestra cualquier ASP.NET que haya presentado al intentar enviar el mensaje de correo electrónico. Sin embargo, ADVERTENCIA: los mensajes de error que ASP.NET notifica cuando no puede enviar un mensaje de correo electrónico pueden ser genéricos. Por ejemplo, si ASP.NET no puede ponerse en contacto con el servidor SMTP (por ejemplo, porque se ha producido un error en el nombre del servidor), el error se `Failure sending mail`.

    > [!NOTE] 
    > 
    > **Importante** Cuando reciba un mensaje de error de un objeto de excepción (`ex` en el código), *no* pase habitualmente ese mensaje a los usuarios. Los objetos de excepción suelen incluir información que los usuarios no deberían ver y que incluso pueden ser una vulnerabilidad de seguridad. Esta es la razón por la que este código incluye la variable `debuggingFlag` que se utiliza como modificador para mostrar el mensaje de error y por qué la variable está establecida de forma predeterminada en false. Debe establecer esa variable en true (y, por tanto, mostrar el mensaje de error) *solo* si tiene problemas para enviar correo electrónico y necesita depurar. Una vez corregidos los problemas, vuelva a establecer `debuggingFlag` en false.

    Modifique la siguiente configuración relacionada con el correo electrónico en el código:

   - Establezca `your-SMTP-host` en el nombre del servidor SMTP al que tenga acceso.
   - Establezca `your-user-name-here` en el nombre de usuario de la cuenta del servidor SMTP.
   - Establezca `your-account-password` en la contraseña de la cuenta del servidor SMTP.
   - Establezca `your-email-address-here` en su propia dirección de correo electrónico. Esta es la dirección de correo electrónico desde la que se envía el mensaje. (Algunos proveedores de correo electrónico no permiten especificar una dirección de `From` diferente y usarán su nombre de usuario como dirección `From`).

     > [!TIP] 
     > 
     > <a id="configuring_email_settings"></a>
     > ### <a name="configuring-email-settings"></a>Configuración del correo electrónico
     > 
     > A veces, puede ser un reto asegurarse de que tiene la configuración correcta para el servidor SMTP, el número de puerto, etc. A continuación se muestran algunas sugerencias:
     > 
     > - El nombre del servidor SMTP suele ser similar `smtp.provider.com` o `smtp.provider.net`. Sin embargo, si publica el sitio en un proveedor de hospedaje, es posible que el nombre del servidor SMTP en ese momento sea `localhost`. Esto se debe a que, una vez que haya publicado y que el sitio se esté ejecutando en el servidor del proveedor, el servidor de correo electrónico podría ser local desde la perspectiva de la aplicación. Este cambio en los nombres de servidor puede significar que debe cambiar el nombre del servidor SMTP como parte del proceso de publicación.
     > - Normalmente, el número de puerto es 25. Sin embargo, algunos proveedores requieren que use el puerto 587 o algún otro puerto.
     > - Asegúrese de usar las credenciales adecuadas. Si ha publicado el sitio en un proveedor de hospedaje, use las credenciales que el proveedor ha indicado específicamente como correo electrónico. Pueden ser diferentes de las credenciales que se usan para publicar.
     > - En ocasiones, no se necesitan credenciales. Si va a enviar correo electrónico mediante su ISP personal, es posible que el proveedor de correo electrónico ya conozca sus credenciales. Después de publicar, es posible que deba usar credenciales diferentes a las que se deben probar en el equipo local.
     > - Si el proveedor de correo electrónico usa el cifrado, debe establecer `WebMail.EnableSsl` en `true`.
4. Ejecute la página *EmailRequest. cshtml* en un explorador. (Asegúrese de que la página esté seleccionada en el área de trabajo **archivos** antes de ejecutarla).
5. Escriba su nombre y una descripción del problema y, a continuación, haga clic en el botón **Enviar** . Se le redirigirá a la página *ProcessRequest. cshtml* , que confirma su mensaje y le envía un mensaje de correo electrónico. 

    ![[imagen]](11-adding-email-to-your-web-site/_static/image2.jpg)

<a id="Sending_a_File"></a>
## <a name="sending-a-file-using-email"></a>Envío de un archivo mediante correo electrónico

También puede enviar archivos adjuntos a mensajes de correo electrónico. En este procedimiento, creará un archivo de texto y dos páginas HTML. Usará el archivo de texto como datos adjuntos de correo electrónico.

1. En el sitio web, agregue un nuevo archivo de texto y asígnele el nombre de *archivo. txt*.
2. Copie el texto siguiente y péguelo en el archivo: 

    `Lorem ipsum dolor sit amet, consectetur adipisicing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat.`
3. Cree una página denominada *sendfile. cshtml* y agregue el marcado siguiente: 

    [!code-html[Main](11-adding-email-to-your-web-site/samples/sample3.html)]
4. Cree una página denominada *ProcessFile. cshtml* y agregue el marcado siguiente: 

    [!code-cshtml[Main](11-adding-email-to-your-web-site/samples/sample4.cshtml)]
5. Modifique la siguiente configuración relacionada con el correo electrónico en el código del ejemplo:

    - Establezca `your-SMTP-host` en el nombre de un servidor SMTP al que tenga acceso.
    - Establezca `your-user-name-here` en el nombre de usuario de la cuenta del servidor SMTP.
    - Establezca `your-email-address-here` en su propia dirección de correo electrónico. Esta es la dirección de correo electrónico desde la que se envía el mensaje.
    - Establezca `your-account-password` en la contraseña de la cuenta del servidor SMTP.
    - Establezca `target-email-address-here` en su propia dirección de correo electrónico. (Como antes, normalmente enviaría un correo electrónico a otra persona, pero para realizar pruebas, puede enviarlo a sí mismo).
6. Ejecute la página *sendfile. cshtml* en un explorador.
7. Escriba su nombre, una línea de asunto y el nombre del archivo de texto que se va a adjuntar (*archivo. txt*).
8. Haga clic en el botón `Submit`. Como antes, se le redirigirá a la página *ProcessFile. cshtml* , que confirma su mensaje y le envía un mensaje de correo electrónico con el archivo adjunto.

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Recursos adicionales

- [Guía de solución de problemas de ASP.NET Web Pages (Razor)](https://go.microsoft.com/fwlink/?LinkId=253001)
- [Protocolo simple de transferencia de correo](https://msdn.microsoft.com/library/aa480435.aspx)
- [Personalizar el comportamiento de todo el sitio para ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=202906)
