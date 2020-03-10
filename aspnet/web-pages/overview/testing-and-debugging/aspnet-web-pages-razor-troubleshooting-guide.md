---
uid: web-pages/overview/testing-and-debugging/aspnet-web-pages-razor-troubleshooting-guide
title: Guía de solución de problemas de ASP.NET Web Pages (Razor) | Microsoft Docs
author: Rick-Anderson
description: En este artículo se describen los problemas que puede tener al trabajar con ASP.NET Web Pages (Razor) y algunas soluciones sugeridas. Versiones de software ASP.NET Web pag...
ms.author: riande
ms.date: 02/10/2014
ms.assetid: 2a2c1833-0bfe-4e2e-9cc0-341b52c7b121
msc.legacyurl: /web-pages/overview/testing-and-debugging/aspnet-web-pages-razor-troubleshooting-guide
msc.type: authoredcontent
ms.openlocfilehash: fc03767c16f46c1e282d24ee3a7df2409a7c38bb
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78473383"
---
# <a name="aspnet-web-pages-razor-troubleshooting-guide"></a>Guía de solución de problemas de ASP.NET Web Pages (Razor)

por [Tom FitzMacken](https://github.com/tfitzmac)

> En este artículo se describen los problemas que puede tener al trabajar con ASP.NET Web Pages (Razor) y algunas soluciones sugeridas.
> 
> ## <a name="software-versions"></a>Versiones de software
> 
> 
> - ASP.NET Web Pages (Razor) 3
>   
> 
> Este tutorial también funciona con ASP.NET Web Pages 2 y ASP.NET Web Pages 1,0.

Este tema contiene las siguientes secciones:

- [Problemas con la ejecución de páginas](#Issues_Running_.cshtml_Pages)
- [Problemas con código Razor](#IssuesWithRazorCode)
- [Problemas con la seguridad y la pertenencia](#membership)
- [Problemas con el envío de correo electrónico](#email)
- [Recursos adicionales](#AdditionalResources)

Para preguntas generales, consulte las preguntas [más frecuentes sobre ASP.NET Web pages (Razor)](https://go.microsoft.com/fwlink/?LinkId=253000).

<a id="Issues_Running_.cshtml_Pages"></a>
## <a name="issues-with-running-pages"></a>Problemas con la ejecución de páginas

Una variedad de problemas puede impedir que las páginas *. cshtml* y *. vbhtml* se ejecuten correctamente. En esta sección se enumeran los mensajes de error comunes y las causas probables.

### <a name="http-error-403---forbidden-access-is-denied"></a>Error HTTP 403-prohibido: acceso denegado

*No tiene permiso para ver este directorio o esta página con las credenciales proporcionadas.*

Este error puede producirse si el servidor no está ejecutando la versión correcta del .NET Framework. Asegúrese de que el equipo que ejecuta el servidor (de forma local o remota) tiene al menos el .NET Framework 4 instalado. Asegúrese también de que la propia aplicación está configurada para ejecutar la versión correcta.

Si ve este problema localmente mientras trabaja en WebMatrix, haga clic en el área de trabajo del **sitio** y, después, en la vista de árbol, haga clic en **configuración**. En la lista **seleccionar versión de .NET Framework** , seleccione **.net 4 (integrado)** . Si ya se ha establecido esta versión, intente ejecutar WebMatrix como administrador.

Asegúrese de que la raíz del sitio web tiene al menos un archivo *. cshtml* .

Si ve este error cuando el servidor Web se encuentra en un servidor remoto, póngase en contacto con el administrador del servidor. Asegúrese de que el servidor tiene instalado el .NET Framework 4 o posterior. Asegúrese también de que la aplicación se está ejecutando en un grupo de aplicaciones que está configurado para usar esa versión de the.NET Framework.

Si tiene control sobre el servidor, asegúrese de que se está ejecutando la versión correcta del .NET Framework. También puede intentar reparar la instalación mediante la ejecución del comando `aspnet_regiis -iru`. (Por ejemplo, si instala IIS después de instalar el .NET Framework, IIS no se configurará correctamente para ejecutar páginas de ASP.NET). Para obtener más información, consulte [ASP.net IIS registration Tool (Aspnet\_regiis. exe)](https://msdn.microsoft.com/library/k6h9cz8h(v=vs.100).aspx).

### <a name="http-error-40314---forbidden"></a>Error HTTP 403,14-prohibido

*El servidor web está configurado para no mostrar el contenido de este directorio.*

Este error puede producirse si se solicita un recurso protegido (como el archivo *Web. config* ) o que se encuentra en una carpeta protegida (como la *aplicación\_datos* o el *código de\_* de la aplicación).

### <a name="http-error-40417---not-found"></a>Error HTTP 404,17-no encontrado

*El contenido solicitado parece ser un script y el controlador de archivos estáticos no lo atenderá.*

Este error puede producirse si el servidor no está configurado correctamente para usar el .NET Framework 4 o posterior y, por tanto, no reconoce el código de los bloques de `@{ }`. Vea la descripción anterior para *http Error 403-prohibido: acceso denegado*.

### <a name="http-error-4047---not-found"></a>Error HTTP 404,7-no encontrado

*El módulo de filtrado de solicitudes está configurado para denegar la extensión de archivo*

Este error puede producirse si se han bloqueado explícitamente las extensiones *. cshtml* o *. vbhtml* en el servidor. Un síntoma de este problema es que las direcciones URL funcionan cuando no incluyen la extensión, pero las direcciones URL que incluyen *. cshtml* o *. vbhtml* no funcionan. Una posible solución consiste en volver a habilitar las extensiones en el archivo *Web. config* del sitio. En el ejemplo siguiente se muestra cómo habilitar la extensión *. cshtml* .

[!code-xml[Main](aspnet-web-pages-razor-troubleshooting-guide/samples/sample1.xml?highlight=5-6)]

### <a name="http-error-4048---not-found"></a>Error HTTP 404,8-no encontrado

*El módulo de filtrado de solicitudes está configurado para denegar una ruta de acceso en la dirección URL que contiene una sección hiddenSegment.*

Este error puede producirse si se solicita un recurso protegido (como el archivo *Web. config* ) o que se encuentra en una carpeta protegida (como la *aplicación\_datos* o el *código de\_* de la aplicación).

### <a name="this-type-of-page-is-not-served-server-error-in--application"></a>No se atiende este tipo de página (error de servidor en la aplicación '/')

Vea la descripción anterior para el error HTTP 404,17.

<a id="IssuesWithRazorCode"></a>
## <a name="issues-with-razor-code"></a>Problemas con código Razor

### <a name="the-name-class-does-not-exist-in-the-current-context"></a>El nombre '*Class*' no existe en el contexto actual

A menudo, un motivo por el que aparece este error es que `class` hace referencia a una aplicación auxiliar, pero el ayudante no está instalado. Por ejemplo, si intenta usar una aplicación auxiliar, pero si no ha instalado el paquete desde NuGet, verá este error. Use la galería de WebMatrix para buscar e instalar el ayudante.

Si el Ayudante está instalado, pero la página todavía no lo reconoce, intente agregar agregar una instrucción `using` al código. En la instrucción `using`, haga referencia al espacio de nombres que incluye el ayudante. Por ejemplo, las aplicaciones auxiliares básicas que se encuentran en el paquete de aplicaciones auxiliares Web de ASP.NET se encuentran en el espacio de nombres `System.Web.Helpers`. En la parte superior de la página en la que desea usar el ayudante, agregue esta línea:

`@using Microsoft.Web.Helpers;`

<a id="membership"></a>
## <a name="issues-with-security-and-membership"></a>Problemas con la seguridad y la pertenencia

Si usa el sistema de seguridad integrado (pertenencia) en ASP.NET Web Pages (Razor), puede encontrar los siguientes problemas.

### <a name="to-call-this-method-the-membershipprovider-property-must-be-an-instance-of-extendedmembershipprovider"></a>Para llamar a este método, la propiedad "Membership. Provider" debe ser una instancia de "ExtendedMembershipProvider".

Este error puede indicar que no hay ninguna clase de `AspNetSqlMembershipProvider` configurada. (Un síntoma es que el sitio funciona correctamente, pero genera este error al publicarlo en el servidor de un proveedor de hospedaje). Una solución para este problema es habilitar explícitamente la pertenencia simple agregando lo siguiente al archivo *Web. config* del sitio:

[!code-xml[Main](aspnet-web-pages-razor-troubleshooting-guide/samples/sample2.xml?highlight=6)]

<a id="email"></a>
## <a name="issues-with-sending-email"></a>Problemas con el envío de correo electrónico

Los problemas con el envío de correo electrónico pueden ser difíciles de depurar. Un problema inicial puede ser que no se puede conectar al servidor SMTP. Si la conexión se realiza correctamente, ASP.NET entrega el mensaje al servidor SMTP. Sin embargo, puede haber problemas con el mensaje que impide que el servidor SMTP lo envíe.

Si su aplicación no envía correo electrónico correctamente, intente lo siguiente:

- El nombre del servidor SMTP suele ser similar `smtp.provider.com` o `smtp.provider.net`. Sin embargo, si publica el sitio en un proveedor de hospedaje, es posible que el nombre del servidor SMTP en ese momento sea `localhost`. Esta situación se produce porque, una vez publicado y el sitio se ejecuta en el servidor del proveedor, el servidor SMTP puede ser local desde la perspectiva de la aplicación. Este cambio en los nombres de servidor puede significar que debe cambiar el nombre del servidor SMTP como parte del proceso de publicación.
- Normalmente, el número de puerto es 25. Sin embargo, algunos proveedores requieren que use el puerto 587 o algún otro puerto. Compruebe con el propietario del servidor SMTP el número de puerto que espera usar.
- Asegúrese de usar las credenciales adecuadas. Si ha publicado el sitio en un proveedor de hospedaje, use las credenciales que el proveedor ha indicado específicamente como correo electrónico. Estas credenciales pueden ser diferentes de las credenciales que se usan para publicar.
- En ocasiones, no se necesitan credenciales. Si va a enviar correo electrónico mediante su ISP personal, es posible que el proveedor de correo electrónico ya conozca sus credenciales. Después de publicar, es posible que deba usar credenciales diferentes a las que se deben probar en el equipo local.
- Si el proveedor de correo electrónico usa el cifrado, establezca `WebMail.EnableSsl` en `true`.

Si se produce un error al enviar correo electrónico, es posible que vea un mensaje de error ASP.NET estándar, que tiene el siguiente aspecto:

![Mensaje de error de ASP.NET cuando hay un problema con el correo electrónico](aspnet-web-pages-razor-troubleshooting-guide/_static/image1.png)

También puede depurar problemas con el envío de correo electrónico mediante el uso de un bloque `try-catch`, como en el ejemplo siguiente. Cuando se usa un bloque `try-catch`, ASP.NET no muestra sus mensajes de error estándar. En su lugar, puede capturar el error en la parte `catch` del bloque.

[!code-cshtml[Main](aspnet-web-pages-razor-troubleshooting-guide/samples/sample3.cshtml)]

Sustituya los valores apropiados por `your-SMTP-server-name`, etc. Entre los mensajes de error que puede ver se incluyen los siguientes:

- *Error al enviar correo.*

    o bien

    *Error en un intento de conexión porque la parte conectada no respondió correctamente después de un período de tiempo, o bien se produjo un error en la conexión establecida porque el host conectado no respondió*

    Este error normalmente significa que la aplicación no se pudo conectar al servidor SMTP. Compruebe el nombre del servidor y el número de puerto.
- *Buzón no disponible. La respuesta del servidor fue: 5.1.0 &lt;someuser@invaliddomain&gt; remitente rechazado: dominio de remitente no válido*

    Este mensaje puede indicar que la dirección `From` no es correcta o falta.
- *La cadena especificada no tiene el formato necesario para una dirección de correo electrónico.*

    Este error podría indicar que el valor de las propiedades `To` o `From` no se reconoce como direcciones de correo electrónico. (ASP.NET no puede comprobar que la dirección de correo electrónico es válida, solo que está en el formato correcto, como *name@domain.com* ).

> [!NOTE]
> Quite el marcado que muestra el error (`@errorMessage`) antes de publicar la página en un sitio activo. No es una buena idea permitir que los usuarios vean los mensajes de error que obtiene de un servidor.

<a id="AdditionalResources"></a>
## <a name="additional-resources"></a>Recursos adicionales

[Preguntas frecuentes de ASP.NET Web Pages (Razor)](https://go.microsoft.com/fwlink/?LinkId=253000)

[WebMatrix y ASP.NET Web pages](https://forums.asp.net/1224.aspx/1?WebMatrix) Forum en el sitio web de ASP.net
