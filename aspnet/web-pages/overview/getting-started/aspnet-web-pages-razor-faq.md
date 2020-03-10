---
uid: web-pages/overview/getting-started/aspnet-web-pages-razor-faq
title: Preguntas más frecuentes sobre ASP.NET Web Pages (Razor) | Microsoft Docs
author: Rick-Anderson
description: En este artículo se enumeran algunas de las preguntas más frecuentes sobre ASP.NET Web Pages (Razor) y WebMatrix. Versiones de software usadas en el tutorial ASP.NET Web Pages (R...
ms.author: riande
ms.date: 02/07/2014
ms.assetid: b137bd04-25e1-47cb-9d96-ef2e179ecf1f
msc.legacyurl: /web-pages/overview/getting-started/aspnet-web-pages-razor-faq
msc.type: authoredcontent
ms.openlocfilehash: 6fa1b668e915af856a693e79eafb7180ed504dc2
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78520657"
---
# <a name="aspnet-web-pages-razor-faq"></a>Preguntas frecuentes de ASP.NET Web Pages (Razor)

por [Tom FitzMacken](https://github.com/tfitzmac)

> > [!NOTE] 
> > WebMatrix ya no se recomienda como un entorno de desarrollo integrado para ASP.NET Web Pages. Use [Visual Studio](xref:aspnet/web-pages/overview/getting-started/program-asp-net-web-pages-in-visual-studio) o [Visual Studio Code](https://code.visualstudio.com/).
>
> En este artículo se enumeran algunas de las preguntas más frecuentes sobre ASP.NET Web Pages (Razor) y WebMatrix.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versiones de software usadas en el tutorial
> 
> 
> - ASP.NET Web Pages (Razor) 3
> - Visual Studio 2013
> - WebMatrix 3
>   
> 
> Este tutorial también funciona con ASP.NET Web Pages 2, WebMatrix 2 y Visual Studio 2012.

- [¿Cuál es la diferencia entre ASP.NET Web Pages, los formularios Web Forms de ASP.NET y ASP.NET MVC?](#Whats_the_difference_between_ASP.NET_Web_Pages,_ASP.NET_Web_Forms,_and_ASP.NET_MVC)
- [¿Necesito WebMatrix para trabajar con páginas web?](#Do_I_need_WebMatrix_in_order_to_work_with_Web_Pages)
- [¿Puedo usar los controles de formularios Web Forms de ASP.NET en una página de páginas web?](#Can_I_use_ASP.NET_Web_Forms_controls_on_a_Web_Pages_page)
- [¿Puedo implementar un sitio de ASP.NET Web Pages sin usar WebMatrix?](#Can_I_deploy_an_ASP.NET_Web_Pages_site_without_using_WebMatrix)
- [¿Tengo que usar la aplicación auxiliar WebSecurity para admitir inicios de sesión?](#Do_I_have_to_use_the_WebSecurity_helper_to_support_logins)
- [¿ASP.NET Web Pages admite HTML5?](#Does_ASP.NET_Web_Pages_support_HTML5)
- [¿Puedo usar JavaScript y jQuery con páginas web?](#Can_I_use_JavaScript_and_jQuery_with_Web_Pages)
- [Recursos adicionales](#AdditionalResources)

Si tiene preguntas sobre errores y otros problemas, consulte la [Guía de solución de problemas de ASP.NET Web pages (Razor)](https://go.microsoft.com/fwlink/?LinkId=253001).

<a id="Whats_the_difference_between_ASP.NET_Web_Pages,_ASP.NET_Web_Forms,_and_ASP.NET_MVC"></a>
## <a name="whats-the-difference-between-aspnet-web-pages-aspnet-web-forms-and-aspnet-mvc"></a>¿Cuál es la diferencia entre ASP.NET Web Pages, los formularios Web Forms de ASP.NET y ASP.NET MVC?

Las tres son tecnologías de ASP.NET para crear aplicaciones web dinámicas:

- ASP.NET Web Pages se centra en agregar código dinámico (en el lado servidor) y acceso a bases de datos a páginas HTML, y ofrece una sintaxis sencilla y ligera.
- Los formularios Web Forms de ASP.NET se basan en un modelo de objetos de página y en los controles de tipo de ventana tradicionales (botones, listas, etc.). Los formularios Web Forms usan un modelo basado en eventos que es familiar para aquellos que han trabajado con el desarrollo basado en el cliente (Windows Forms).
- ASP.NET MVC implementa el patrón de controlador de vista de modelo para ASP.NET. El énfasis está en "separación de preocupaciones" (procesamiento, datos y capas de la interfaz de usuario).

Los tres marcos son totalmente compatibles y continúan siendo desarrollados por el equipo de ASP.NET. En general, la elección del marco de trabajo que se va a usar depende del fondo y la experiencia con ASP.NET.

En concreto, ASP.NET Web Pages se diseñó para facilitar a los usuarios que ya conocen HTML agregar procesamiento de servidor a sus páginas. Es una buena elección para estudiantes, aficionados, personas en general que no están familiarizados con la programación. También puede ser una buena elección para los desarrolladores que tienen experiencia con las tecnologías Web de non-ASP.NET.

<a id="Do_I_need_WebMatrix_in_order_to_work_with_Web_Pages"></a>
## <a name="do-i-need-webmatrix-in-order-to-work-with-web-pages"></a>¿Necesito WebMatrix para trabajar con páginas web?

No. WebMatrix ya no se recomienda como un entorno de desarrollo integrado para ASP.NET Web Pages. Use [Visual Studio](program-asp-net-web-pages-in-visual-studio.md) o [Visual Studio Code](https://code.visualstudio.com/).

Si no desea usar Visual Studio ni Visual Studio Code, puede instalar los productos de componentes individualmente mediante [instalador de plataforma web de Microsoft](https://www.microsoft.com/web/downloads/platform.aspx). Necesita los siguientes productos:

- Microsoft .NET Framework 4.5
- ASP.NET MVC 5 (que instala también el marco de ASP.NET Web Pages)
- IIS Express (el servidor Web)
- Microsoft SQL Server Compact 4,0 (la base de datos)

Puede usar un editor de texto para editar páginas *. cshtml* (o *. vbhtml*).

La administración de SQL Server Compact bases de datos (archivos *. sdf* ) sin una herramienta es un poco más difícil. Herramientas de containds de Visual Studio para administrar bases de datos *. sdf* . También puede ejecutar comandos SQL en el código para realizar muchas tareas de administración de SQL Server.

Para probar las páginas *. cshtml* sin usar un entorno de desarrollo integrado (IDE), puede implementarlas en un servidor activo. (Consulte [¿puedo implementar un sitio de ASP.NET Web pages sin usar WebMatrix?](#Can_I_deploy_an_ASP.NET_Web_Pages_site_without_using_WebMatrix))

### <a name="running-iis-express-without-using-an-ide"></a>Ejecutar IIS Express sin usar un IDE

Si instala IIS Express en el equipo como un servidor Web, puede utilizarlo para probar las páginas. Puede ejecutar IIS Express desde la línea de comandos y asociarlo a un número de puerto específico. A continuación, especifique ese puerto cuando solicite archivos *. cshtml* en el explorador.

En Windows, abra un símbolo del sistema con privilegios de administrador y cambie a *c:\Archivos de Programa\iis Express.* (Para los sistemas de 64 bits, use la carpeta C:\Archivos de programa *(x86) \IIS Express).* A continuación, escriba el siguiente comando con la ruta de acceso real a su sitio:

`iisexpress.exe /port:35896 /path:C:\BasicWebSite`

Puede usar cualquier número de puerto que aún no esté reservado por otro proceso. (Los números de puerto por encima de 1024 suelen ser gratis). Para el valor `path`, use la ruta de acceso de la carpeta del sitio web donde se encuentran los archivos *. cshtml* .

Después de ejecutar este comando para configurar IIS Express para atender las páginas, puede abrir un explorador y buscar un archivo *. cshtml* . Use una dirección URL similar a la siguiente:

`http://localhost:35896/default.cshtml`

Para obtener ayuda con las opciones de la línea de comandos de IIS Express, escriba `iisexpress.exe /?` en la línea de comandos.

<a id="Can_I_use_ASP.NET_Web_Forms_controls_on_a_Web_Pages_page"></a>
## <a name="can-i-use-aspnet-web-forms-controls-on-a-web-pages-page"></a>¿Puedo usar los controles de formularios Web Forms de ASP.NET en una página de páginas web?

No. Los controles de formularios Web Forms como el control [CheckBox](https://msdn.microsoft.com/library/system.web.ui.webcontrols.checkbox) , los [controles de validación](https://msdn.microsoft.com/library/bwd43d0x)y el control [GridView](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview) solo funcionan en páginas de formularios Web Forms (archivos *. aspx* ). Estos controles requieren el marco de trabajo de la página de formularios Web Forms.

<a id="Can_I_deploy_an_ASP.NET_Web_Pages_site_without_using_WebMatrix"></a>
## <a name="can-i-deploy-an-aspnet-web-pages-site-without-using-webmatrix"></a>¿Puedo implementar un sitio de ASP.NET Web Pages sin usar WebMatrix?

Sí. Puede copiar manualmente los archivos del sitio web en un servidor (normalmente mediante FTP). Si realiza una copia manual, también tiene que copiar los archivos que admiten SQL Server Compact (la base de datos). Para obtener más información, consulte la entrada de blog sobre la [implementación de aplicaciones Web pages sin una herramienta](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2317).

<a id="Do_I_have_to_use_the_WebSecurity_helper_to_support_logins"></a>
## <a name="do-i-have-to-use-the-websecurity-helper-to-support-logins"></a>¿Tengo que usar la aplicación auxiliar WebSecurity para admitir inicios de sesión?

No. El proveedor de `SimpleMembership` que forma parte de ASP.NET Web Pages es una opción. También están disponibles los proveedores de seguridad que forman parte de ASP.NET (que puede usar para trabajar con formularios Web Forms). Por ejemplo, puede utilizar la autenticación de formularios en ASP.NET Web Pages tal como lo haría en formularios Web Forms. Para ver un ejemplo de cómo usar la autenticación de formularios, vea el Soporte técnico de Microsoft artículo [cómo implementar la autenticación basada en formularios en la aplicación ASP.net C#mediante .net](https://support.microsoft.com/kb/301240). Para descargar un ejemplo sencillo, consulte la [versión de ASP.net de inicio de sesión &amp; contraseña](http://www.codeguru.com/csharp/.net/net_asp/scripting/article.php/c19295/ASPNET-version-of-Login--Password.htm).

Para obtener información sobre cómo usar la autenticación de Windows, consulte la entrada de blog [uso de la autenticación de Windows en ASP.NET Web pages](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2298).

<a id="Does_ASP.NET_Web_Pages_support_HTML5"></a>
## <a name="does-aspnet-web-pages-support-html5"></a>¿ASP.NET Web Pages admite HTML5?

Sí. Las páginas que crea con ASP.NET Web Pages (páginas *. cshtml* o *. vbhtml* ) son esencialmente páginas HTML que también contienen código que se ejecuta en el servidor, antes de que se represente la página. Siempre que el explorador del usuario admita HTML5, puede usar elementos HTML5 en una página *. cshtml* o *. vbhtml* .

<a id="Can_I_use_JavaScript_and_jQuery_with_Web_Pages"></a>
## <a name="can-i-use-javascript-and-jquery-with-web-pages"></a>¿Puedo usar JavaScript y jQuery con páginas web?

Por supuesto. Las páginas que crea con ASP.NET Web Pages (páginas *. cshtml* o *. vbhtml* ) son simplemente páginas HTML con código de servidor. Por lo tanto, todo lo que puede hacer en una página HTML normal mediante JavaScript o jQuery también puede hacerlo en una página *. cshtml* o *. vbhtml* .

La plantilla del **sitio de inicio** de WebMatrix contiene varias bibliotecas de jQuery. Si crea un sitio mediante esa plantilla, la carpeta *scripts* contiene una biblioteca principal de jQuery (*jQuery-1.6.2. js)* y bibliotecas para la validación de jQuery (*jQuery. Validate. js*, etc.).

A continuación se muestran algunas entradas de blog que muestran cómo usar jQuery con ASP.NET Web Pages:

- [Agregar jQuery maravillas a ASP.NET Web pages mediante WebMatrix](http://rachelappel.com/jquery/adding-jquery-goodness-to-asp-net-web-pages-using-webmatrix/) mediante Rachel Appel
- [5 min: WebMatrix + jQuery UI + JSON + plantillas de jQuery](http://joeriks.com/2011/01/30/5-min-webmatrix-jquery-ui-json-jquery-templates/) por Jonas Eriksson
- [Formularios de WebMatrix y jQuery](http://mikesdotnetting.com/Article/155/WebMatrix-And-jQuery-Forms) de Mike

<a id="AdditionalResources"></a>
## <a name="additional-resources"></a>Recursos adicionales

[Guía de solución de problemas de ASP.NET Web Pages (Razor)](https://go.microsoft.com/fwlink/?LinkId=253001)

[WebMatrix y ASP.NET Web pages Forum](https://forums.asp.net/1224.aspx/1?WebMatrix) en el sitio web de ASP.net
