---
uid: web-pages/overview/ui-layouts-and-themes/18-customizing-site-wide-behavior
title: Personalizar el comportamiento de todo el sitio para sitios de ASP.NET Web Pages (Razor) | Microsoft Docs
author: Rick-Anderson
description: En este capítulo se explica cómo establecer la configuración en todo el sitio web o en una carpeta completa, en lugar de simplemente en una página.
ms.author: riande
ms.date: 02/17/2014
ms.assetid: e158bed7-226f-4275-b02e-7553bd58c669
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/18-customizing-site-wide-behavior
msc.type: authoredcontent
ms.openlocfilehash: f05e05f725d9209bce283ce18659ae5efe4de2ee
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78515251"
---
# <a name="customizing-site-wide-behavior-for-aspnet-web-pages-razor-sites"></a>Personalización del comportamiento en todo el sitio para sitios de ASP.NET Web Pages (Razor)

por [Tom FitzMacken](https://github.com/tfitzmac)

> En este artículo se explica cómo crear la configuración del sitio para las páginas en un sitio web de ASP.NET Web Pages (Razor).
> 
> Temas que se abordarán:
> 
> - Cómo ejecutar código que le permite establecer valores (valores globales o valores auxiliares) para todas las páginas de un sitio.
> - Cómo ejecutar código que le permite establecer los valores de todas las páginas de una carpeta.
> - Cómo ejecutar código antes y después de que se cargue una página.
> - Cómo enviar errores a una página de error central.
> - Cómo agregar autenticación a todas las páginas de una carpeta.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versiones de software usadas en el tutorial
> 
> 
> - ASP.NET Web Pages (Razor) 2
> - WebMatrix 3
> - Biblioteca de aplicaciones auxiliares Web de ASP.NET (paquete NuGet)
>   
> 
> Este tutorial también funciona con ASP.NET Web Pages 3 y Visual Studio 2013 (o Visual Studio Express 2013 para web), salvo que no se puede usar la biblioteca de aplicaciones auxiliares Web de ASP.NET.

<a id="Adding_Website_Startup_Code"></a>
## <a name="adding-website-startup-code-for-aspnet-web-pages"></a>Agregar código de inicio del sitio web para ASP.NET Web Pages

En gran parte del código que se escribe en ASP.NET Web Pages, una página individual puede contener todo el código necesario para esa página. Por ejemplo, si una página envía un mensaje de correo electrónico, es posible colocar todo el código para esa operación en una sola página. Esto puede incluir el código para inicializar la configuración para enviar correo electrónico (es decir, para el servidor SMTP) y para enviar el mensaje de correo electrónico.

Sin embargo, en algunas situaciones, puede que desee ejecutar código antes de que se ejecute cualquier página del sitio. Esto resulta útil para establecer los valores que se pueden usar en cualquier parte del sitio (denominados *valores globales*). Por ejemplo, algunas aplicaciones auxiliares requieren que proporcione valores como la configuración de correo electrónico o las claves de cuenta. Puede ser útil para mantener esta configuración en valores globales.

Para ello, puede crear una página denominada *\_AppStart. cshtml* en la raíz del sitio. Si esta página existe, se ejecuta la primera vez que se solicita una página del sitio. Por lo tanto, es un buen lugar para ejecutar código con el fin de establecer valores globales. (Dado que *\_AppStart. cshtml* tiene un prefijo de subrayado, ASP.net no enviará la página a un explorador aunque los usuarios la soliciten directamente).

En el diagrama siguiente se muestra cómo funciona la página de *\_AppStart. cshtml* . Cuando llega una solicitud para una página y se trata de la primera solicitud de cualquier página del sitio, ASP.NET comprueba primero si existe una página de *\_AppStart. cshtml* . En ese caso, se ejecuta cualquier código de la página *\_AppStart. cshtml* y, a continuación, se ejecuta la página solicitada.

![[imagen]](18-customizing-site-wide-behavior/_static/image1.jpg)

## <a name="setting-global-values-for-your-website"></a>Establecer valores globales para el sitio web

1. En la carpeta raíz de un sitio web de WebMatrix, cree un archivo denominado *\_AppStart. cshtml*. El archivo debe estar en la raíz del sitio.
2. Reemplace el contenido existente por lo siguiente: 

    [!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample1.cshtml)]

    Este código almacena un valor en el Diccionario de `AppState`, que está disponible automáticamente para todas las páginas del sitio. Tenga en cuenta que el archivo *\_AppStart. cshtml* no tiene ningún marcado. La página ejecutará el código y, a continuación, redirigirá a la página que se solicitó originalmente.

    > [!NOTE]
    > Tenga cuidado al colocar el código en el archivo *\_AppStart. cshtml* . Si se produce algún error en el código del archivo *\_AppStart. cshtml* , el sitio web no se iniciará.
3. En la carpeta raíz, cree una nueva página denominada *AppName. cshtml*.
4. Reemplace el marcado y el código predeterminados por lo siguiente: 

    [!code-html[Main](18-customizing-site-wide-behavior/samples/sample2.html)]

    Este código extrae el valor del objeto `AppState` que estableció en la página *\_AppStart. cshtml* .
5. Ejecute la página *AppName. cshtml* en un explorador. (Asegúrese de que la página esté seleccionada en el área de trabajo **archivos** antes de ejecutarla). La página muestra el valor global. 

    ![[imagen]](18-customizing-site-wide-behavior/_static/image2.jpg)

<a id="Setting_Values_For_Helpers"></a>
## <a name="setting-values-for-helpers"></a>Establecer valores para aplicaciones auxiliares

Un buen uso para el archivo *\_AppStart. cshtml* es establecer los valores de las aplicaciones auxiliares que se usan en el sitio y que se deben inicializar. Algunos ejemplos típicos son la configuración de correo electrónico de la aplicación auxiliar de `WebMail` y las claves privadas y públicas de la aplicación auxiliar de `ReCaptcha`. En casos como estos, puede establecer los valores una vez en el *\_AppStart. cshtml* y, a continuación, ya están establecidos para todas las páginas del sitio.

En este procedimiento se muestra cómo establecer la configuración de `WebMail` globalmente. (Para obtener más información acerca del uso de la aplicación auxiliar de `WebMail`, consulte [Agregar correo electrónico a un sitio de ASP.NET Web pages](../getting-started/11-adding-email-to-your-web-site.md)).

1. Agregue la biblioteca de aplicaciones auxiliares Web de ASP.NET a su sitio web, tal como se describe en [instalación de aplicaciones auxiliares en un sitio de ASP.NET Web pages](https://go.microsoft.com/fwlink/?LinkId=252372), si aún no lo ha hecho.
2. Si aún no tiene un archivo *\_AppStart. cshtml* , en la carpeta raíz de un sitio web, cree un archivo denominado *\_AppStart. cshtml*.
3. Agregue la siguiente configuración de `WebMail` al archivo *\_AppStart. cshtml* : 

    [!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample3.cshtml?highlight=2-7)]

    Modifique la siguiente configuración relacionada con el correo electrónico en el código:

   - Establezca `your-SMTP-host` en el nombre del servidor SMTP al que tenga acceso.
   - Establezca `your-user-name-here` en el nombre de usuario de la cuenta del servidor SMTP.
   - Establezca `your-account-password` en la contraseña de la cuenta del servidor SMTP.
   - Establezca `your-email-address-here` en su propia dirección de correo electrónico. Esta es la dirección de correo electrónico desde la que se envía el mensaje. (Algunos proveedores de correo electrónico no permiten especificar una dirección de `From` diferente y usarán su nombre de usuario como dirección `From`).

     Para obtener más información acerca de la configuración de SMTP, consulte Configuración del [correo](https://go.microsoft.com/fwlink/?LinkID=202899#configuring_email_settings) electrónico en el artículo [envío de correo electrónico desde un sitio de ASP.NET Web pages (Razor)](https://go.microsoft.com/fwlink/?LinkID=202899) y [problemas con el envío de correo electrónico](https://go.microsoft.com/fwlink/?LinkId=253001#email) en la [Guía de solución de problemas de ASP.NET Web pages (Razor)](https://go.microsoft.com/fwlink/?LinkId=253001).
4. Guarde el archivo *\_AppStart. cshtml* y ciérrelo.
5. En la carpeta raíz de un sitio web, cree una nueva página denominada *TestEmail. cshtml*.
6. Reemplace el contenido existente por lo siguiente: 

     [!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample4.cshtml)]
7. Ejecute la página *TestEmail. cshtml* en un explorador.
8. Rellene los campos para enviarse un mensaje de correo electrónico y, a continuación, haga clic en **Enviar**.
9. Compruebe su correo electrónico para asegurarse de que ha recibido el mensaje.

La parte importante de este ejemplo es que la configuración que no suele cambiar, como el nombre del servidor SMTP y las credenciales de correo electrónico, se establece en el archivo *\_AppStart. cshtml* . De este modo, no tendrá que volver a configurarlos en cada página en la que envíe el correo electrónico. (Sin embargo, si por alguna razón necesita cambiar esta configuración, puede establecerla individualmente en una página). En la página, solo se establecen los valores que normalmente cambian cada vez, como el destinatario y el cuerpo del mensaje de correo electrónico.

<a id="Running_Code_Before_and_After"></a>
## <a name="running-code-before-and-after-files-in-a-folder"></a>Ejecutar código antes y después de los archivos de una carpeta

Del mismo modo que puede usar *\_AppStart. cshtml* para escribir código antes de que se ejecuten las páginas del sitio, puede escribir código que se ejecute antes (y después) cualquier página de una carpeta determinada que se ejecute. Esto resulta útil para cosas como establecer la misma página de diseño para todas las páginas de una carpeta, o para comprobar que un usuario ha iniciado sesión antes de ejecutar una página en la carpeta.

En el caso de las páginas de carpetas concretas, puede crear código en un archivo denominado *\_PageStart. cshtml*. En el diagrama siguiente se muestra cómo funciona la página *\_PageStart. cshtml* . Cuando llega una solicitud para una página, ASP.NET busca primero una página de *\_AppStart. cshtml* y la ejecuta. A continuación, ASP.NET comprueba si hay una\_página de *PageStart. cshtml* y, en caso afirmativo, la ejecuta. A continuación, ejecuta la página solicitada.

Dentro de la página *\_PageStart. cshtml* , puede especificar dónde desea que se ejecute la página solicitada, incluyendo un método `RunPage`. Esto le permite ejecutar código antes de que se ejecute la página solicitada y, después, otra vez. Si no incluye `RunPage`, se ejecuta todo el código de *\_PageStart. cshtml* y, a continuación, la página solicitada se ejecuta automáticamente.

![[imagen]](18-customizing-site-wide-behavior/_static/image3.jpg)

ASP.NET permite crear una jerarquía de archivos *\_PageStart. cshtml* . Puede colocar un *\_archivo PageStart. cshtml* en la raíz del sitio y en cualquier subcarpeta. Cuando se solicita una página, se ejecuta la *\_archivo PageStart. cshtml* en el nivel superior (más cercano a la raíz del sitio), seguido del *\_archivo PageStart. cshtml* en la subcarpeta siguiente, y así sucesivamente en la estructura de la subcarpeta hasta que la solicitud llega a la carpeta que contiene la página solicitada. Una vez que se han ejecutado todos los archivos *\_PageStart. cshtml* correspondientes, se ejecuta la página solicitada.

Por ejemplo, podría tener la siguiente combinación de *\_archivos PageStart. cshtml* y el archivo *default. cshtml* :

[!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample5.cshtml)]

[!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample6.cshtml)]

[!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample7.cshtml)]

Al ejecutar */MyFolder/default.cshtml*, verá lo siguiente:

[!code-console[Main](18-customizing-site-wide-behavior/samples/sample8.cmd)]

## <a name="running-initialization-code-for-all-pages-in-a-folder"></a>Ejecutar código de inicialización para todas las páginas de una carpeta

Un buen uso de *\_archivos PageStart. cshtml* es inicializar la misma página de diseño para todos los archivos de una sola carpeta.

1. En la carpeta raíz, cree una nueva carpeta denominada *InitPages*.
2. En la carpeta *InitPages* de su sitio web, cree un archivo denominado *\_PageStart. cshtml* y reemplace el marcado y el código predeterminados por lo siguiente: 

    [!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample9.cshtml)]
3. En la raíz del sitio web, cree una carpeta denominada *Shared*.
4. En la carpeta *compartida* , cree un archivo denominado *\_Layout1. cshtml* y reemplace el marcado y el código predeterminados por lo siguiente: 

    [!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample10.cshtml)]
5. En la carpeta *InitPages* , cree un archivo denominado *Content1. cshtml* y reemplace el contenido existente por lo siguiente: 

    [!code-html[Main](18-customizing-site-wide-behavior/samples/sample11.html)]
6. En la carpeta *InitPages* , cree otro archivo denominado *Content2. cshtml* y reemplace el marcado predeterminado por lo siguiente: 

    [!code-html[Main](18-customizing-site-wide-behavior/samples/sample12.html)]
7. Ejecute *Content1. cshtml* en un explorador. 

    ![[imagen]](18-customizing-site-wide-behavior/_static/image4.jpg)

    Cuando se ejecuta la página *Content1. cshtml* , el archivo *\_PageStart. cshtml* establece `Layout` y también establece `PageData["MyBackground"]` en un color. En *Content1. cshtml*, se aplican el diseño y el color.
8. Mostrar *Content2. cshtml* en un explorador. 

    El diseño es el mismo, ya que ambas páginas usan la misma página de diseño y el mismo color que se inicializaron en *\_PageStart. cshtml*.

## <a name="using-_pagestartcshtml-to-handle-errors"></a>Uso de \_PageStart. cshtml para controlar errores

Otro buen uso de la *\_archivo PageStart. cshtml* es crear una manera de controlar los errores de programación (excepciones) que pueden producirse en cualquier página *. cshtml* de una carpeta. En este ejemplo se muestra una manera de hacerlo.

1. En la carpeta raíz, cree una carpeta denominada *InitCatch*.
2. En la carpeta *InitCatch* de su sitio web, cree un archivo denominado *\_PageStart. cshtml* y reemplace el marcado y el código existentes por lo siguiente: 

    [!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample13.cshtml)]

    En este código, intente ejecutar la página solicitada explícitamente llamando al método `RunPage` dentro de un bloque `try`. Si se produce algún error de programación en la página solicitada, se ejecuta el código dentro del bloque `catch`. En este caso, el código redirige a una página (*error. cshtml*) y pasa el nombre del archivo que experimentó el error como parte de la dirección URL. (Creará la página en breve).
3. En la carpeta *InitCatch* de su sitio web, cree un archivo denominado *Exception. cshtml* y reemplace el marcado y el código existentes por lo siguiente: 

    [!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample14.cshtml)]

    Para los fines de este ejemplo, lo que está haciendo en esta página es la creación deliberada de un error al intentar abrir un archivo de base de datos que no existe.
4. En la carpeta raíz, cree un archivo denominado *error. cshtml* y reemplace el marcado y el código existentes por lo siguiente: 

    [!code-html[Main](18-customizing-site-wide-behavior/samples/sample15.html)]

    En esta página, la expresión `@Request["source"]` obtiene el valor de la dirección URL y lo muestra.
5. En la barra de herramientas, haga clic en **Guardar**.
6. Ejecute *Exception. cshtml* en un explorador. 

    ![[imagen]](18-customizing-site-wide-behavior/_static/image5.jpg)

    Dado que se produce un error en *Exception. cshtml*, la página *\_PageStart. cshtml* redirige al archivo *error. cshtml* , que muestra el mensaje.

    Para obtener más información sobre las excepciones, vea [Introducción a la programación de ASP.NET Web pages mediante la sintaxis Razor](https://go.microsoft.com/fwlink/?LinkID=251587).

<a id="Using__PageStart.cshtml_to_Restrict_Folder_Access"></a>
## <a name="using-_pagestartcshtml-to-restrict-folder-access"></a>Usar \_PageStart. cshtml para restringir el acceso a las carpetas

También puede usar el archivo *\_PageStart. cshtml* para restringir el acceso a todos los archivos de una carpeta.

1. En WebMatrix, cree un nuevo sitio web con la opción **sitio a partir de plantilla** .
2. En plantillas disponibles, seleccione **sitio de inicio**.
3. En la carpeta raíz, cree una carpeta denominada *AuthenticatedContent*.
4. En la carpeta *AuthenticatedContent* , cree un archivo denominado *\_PageStart. cshtml* y reemplace el marcado y el código existentes por lo siguiente: 

    [!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample16.cshtml)]

    El código comienza evitando que se almacenen en caché todos los archivos de la carpeta. (Esto es necesario para escenarios como equipos públicos, donde no desea que las páginas almacenadas en caché de un usuario estén disponibles para el siguiente usuario). A continuación, el código determina si el usuario ha iniciado sesión en el sitio para que pueda ver cualquiera de las páginas de la carpeta. Si el usuario no ha iniciado sesión, el código redirige a la página de inicio de sesión. La página de inicio de sesión puede devolver al usuario a la página que se solicitó originalmente si incluye un valor de cadena de consulta denominado `ReturnUrl`.
5. Cree una nueva página en la carpeta *AuthenticatedContent* denominada *Page. cshtml*.
6. Reemplace el marcado predeterminado por lo siguiente:  

    [!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample17.cshtml)]
7. Ejecute *Page. cshtml* en un explorador. El código le redirige a una página de inicio de sesión. Debe registrarse antes de iniciar sesión. Después de haber registrado y iniciado sesión, puede navegar a la página y ver su contenido.

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Recursos adicionales

[Introducción a la programación de ASP.NET Web Pages mediante la sintaxis Razor](https://go.microsoft.com/fwlink/?LinkID=251587)
