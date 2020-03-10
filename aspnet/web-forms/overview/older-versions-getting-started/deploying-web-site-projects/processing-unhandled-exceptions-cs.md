---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/processing-unhandled-exceptions-cs
title: Procesar excepciones no controladas (C#) | Microsoft Docs
author: rick-anderson
description: Cuando se produce un error de tiempo de ejecución en una aplicación web en producción, es importante notificar a un desarrollador y registrar el error para que se pueda diagnosticar en la...
ms.author: riande
ms.date: 06/09/2009
ms.assetid: 5bc1afd5-2484-4528-b158-ab218ba150e8
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/processing-unhandled-exceptions-cs
msc.type: authoredcontent
ms.openlocfilehash: 27d827238d944f86cd913d2b8ecd12729b99f391
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78510661"
---
# <a name="processing-unhandled-exceptions-c"></a>Procesar excepciones no controladas (C#)

por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Vea o descargue el código de ejemplo](https://github.com/dotnet/AspNetDocs/tree/master/aspnet/web-forms/overview/older-versions-getting-started/deploying-web-site-projects/processing-unhandled-exceptions-cs/samples) ([cómo descargarlo](/aspnet/core/tutorials/index#how-to-download-a-sample))

> Cuando se produce un error de tiempo de ejecución en una aplicación web en producción, es importante notificar a un desarrollador y registrar el error para que se pueda diagnosticar en un momento posterior. En este tutorial se proporciona información general sobre cómo ASP.NET procesa los errores en tiempo de ejecución y examina una manera de ejecutar código personalizado siempre que una excepción no controlada se propaga hasta el tiempo de ejecución de ASP.NET.

## <a name="introduction"></a>Introducción

Cuando se produce una excepción no controlada en una aplicación ASP.NET, se propaga hasta el tiempo de ejecución de ASP.NET, que genera el evento `Error` y muestra la página de error adecuada. Hay tres tipos diferentes de páginas de error: la pantalla amarilla de error en tiempo de ejecución de muerte (YSOD). los detalles de la excepción YSOD; y páginas de error personalizadas. En el [tutorial anterior](displaying-a-custom-error-page-cs.md) , se configuró la aplicación para usar una página de error personalizada para los usuarios remotos y los detalles de la excepción YSOD para los usuarios que visitan localmente.

El uso de una página de error personalizada fácil de usar que coincida con la apariencia y el funcionamiento del sitio es preferible al error de tiempo de ejecución predeterminado YSOD, pero mostrar una página de error personalizada es solo una parte de una solución de control de errores completa. Cuando se produce un error en una aplicación de producción, es importante que los desarrolladores reciban una notificación del error para que puedan deshacer la causa de la excepción y solucionarlo. Además, se deben registrar los detalles del error para que se pueda examinar y diagnosticar el error en un momento posterior.

En este tutorial se muestra cómo obtener acceso a los detalles de una excepción no controlada para que se puedan registrar y que se notifique a un desarrollador. En los dos tutoriales siguientes se exploran las bibliotecas de registro de errores que, después de un poco de configuración, notificarán automáticamente a los desarrolladores de errores en tiempo de ejecución y registrarán sus detalles.

> [!NOTE]
> La información examinada en este tutorial es muy útil si necesita procesar las excepciones no controladas de alguna manera única o personalizada. En los casos en los que solo tiene que registrar la excepción y notificar a un desarrollador, el uso de una biblioteca de registro de errores es la manera de ir. Los dos tutoriales siguientes proporcionan una visión general de las dos bibliotecas de este tipo.

## <a name="executing-code-when-theerrorevent-is-raised"></a>Ejecutar código cuando se genera el evento`Error`

Los eventos proporcionan a un objeto un mecanismo para señalar que se ha producido algo interesante y para que otro objeto ejecute código en respuesta. Como desarrollador de ASP.NET, está acostumbrado a pensar en términos de eventos. Si desea ejecutar código cuando el visitante haga clic en un botón determinado, cree un controlador de eventos para el evento de `Click` de ese botón y coloque el código allí. Dado que el tiempo de ejecución de ASP.NET genera su [evento`Error`](https://msdn.microsoft.com/library/system.web.httpapplication.error.aspx) cada vez que se produce una excepción no controlada, se sigue el código para registrar los detalles del error en un controlador de eventos. Pero ¿cómo se crea un controlador de eventos para el evento `Error`?

El evento `Error` es uno de los muchos eventos de la [clase`HttpApplication`](https://msdn.microsoft.com/library/system.web.httpapplication.aspx) que se producen en ciertas fases de la canalización http durante la vigencia de una solicitud. Por ejemplo, el [evento`BeginRequest`](https://msdn.microsoft.com/library/system.web.httpapplication.beginrequest.aspx) de la clase `HttpApplication` se genera al inicio de cada solicitud; su [evento`AuthenticateRequest`](https://msdn.microsoft.com/library/system.web.httpapplication.authenticaterequest.aspx) se genera cuando un módulo de seguridad ha identificado el solicitante. Estos eventos `HttpApplication` proporcionan al desarrollador de páginas un medio para ejecutar la lógica personalizada en los distintos puntos de la duración de una solicitud.

Los controladores de eventos para los eventos de `HttpApplication` se pueden colocar en un archivo especial denominado `Global.asax`. Para crear este archivo en el sitio web, agregue un nuevo elemento a la raíz de su sitio web con la plantilla de clase de aplicación global con el nombre `Global.asax`.

[![](processing-unhandled-exceptions-cs/_static/image2.png)](processing-unhandled-exceptions-cs/_static/image1.png)

**Figura 1**: agregar `Global.asax` a la aplicación Web  
([Haga clic para ver la imagen de tamaño completo](processing-unhandled-exceptions-cs/_static/image3.png))

El contenido y la estructura del archivo `Global.asax` creado por Visual Studio difieren ligeramente en función de si se usa un proyecto de aplicación web (WAP) o un proyecto de sitio web (WSP). Con WAP, el `Global.asax` se implementa como dos archivos independientes: `Global.asax` y `Global.asax.cs`. El archivo `Global.asax` no contiene nada excepto una directiva `@Application` que haga referencia al archivo `.cs`; los controladores de eventos de interés se definen en el archivo de `Global.asax.cs`. En el caso de WSPs, solo se crea un único archivo, `Global.asax`, y los controladores de eventos se definen en un bloque de `<script runat="server">`.

El archivo `Global.asax` creado en una plantilla de clase de aplicación global de Visual Studio incluye controladores de eventos denominados `Application_BeginRequest`, `Application_AuthenticateRequest`y `Application_Error`, que son controladores de eventos para los eventos `HttpApplication` `BeginRequest`, `AuthenticateRequest`y `Error`, respectivamente. También hay controladores de eventos denominados `Application_Start`, `Session_Start`, `Application_End`y `Session_End`, que son controladores de eventos que se activan cuando se inicia la aplicación Web, cuando se inicia una nueva sesión, cuando la aplicación finaliza y cuando finaliza una sesión, respectivamente. El archivo `Global.asax` creado en un WSP por Visual Studio solo contiene los controladores de eventos `Application_Error`, `Application_Start`, `Session_Start`, `Application_End`y `Session_End`.

> [!NOTE]
> Al implementar la aplicación ASP.NET, debe copiar el archivo de `Global.asax` en el entorno de producción. No es necesario copiar el archivo de `Global.asax.cs`, que se crea en WAP, en producción, ya que este código se compila en el ensamblado del proyecto.

Los controladores de eventos creados por la plantilla de clase de aplicación global de Visual Studio no son exhaustivos. Puede Agregar un controlador de eventos para cualquier `HttpApplication` evento mediante el nombre del `Application_EventName`del controlador de eventos. Por ejemplo, puede Agregar el código siguiente al archivo `Global.asax` para crear un controlador de eventos para el [evento`AuthorizeRequest`](https://msdn.microsoft.com/library/system.web.httpapplication.authorizerequest.aspx):

[!code-cs[Main](processing-unhandled-exceptions-cs/samples/sample1.cs)]

Del mismo modo, puede quitar los controladores de eventos creados por la plantilla de clase de aplicación global que no son necesarios. En este tutorial, solo se necesita un controlador de eventos para el evento `Error`. no dude en quitar los otros controladores de eventos del archivo de `Global.asax`.

> [!NOTE]
> Los *módulos http* ofrecen otra manera de definir controladores de eventos para eventos de `HttpApplication`. Los módulos HTTP se crean como un archivo de clase que puede colocarse directamente en el proyecto de aplicación web o dividirse en una biblioteca de clases independiente. Dado que se pueden separar en una biblioteca de clases, los módulos HTTP ofrecen un modelo más flexible y reutilizable para crear `HttpApplication` controladores de eventos. Mientras que el archivo `Global.asax` es específico de la aplicación web donde reside, los módulos HTTP se pueden compilar en ensamblados, momento en el que agregar el módulo HTTP a un sitio web es tan sencillo como quitar el ensamblado en la carpeta `Bin` y registrar el módulo en `Web.config`. En este tutorial no se examina la creación y el uso de módulos HTTP, pero las dos bibliotecas de registro de errores que se usan en los dos tutoriales siguientes se implementan como módulos HTTP. Para obtener más información sobre las ventajas de los módulos HTTP [, consulte uso de módulos y controladores http para crear componentes ASP.net conectables](https://msdn.microsoft.com/library/aa479332.aspx).

## <a name="retrieving-information-about-the-unhandled-exception"></a>Recuperar información sobre la excepción no controlada

En este momento, tenemos un archivo global. asax con un controlador de eventos `Application_Error`. Cuando se ejecuta este controlador de eventos, es necesario notificar al desarrollador el error y registrar los detalles. Para realizar estas tareas, primero debemos determinar los detalles de la excepción que se generó. Utilice el [método`GetLastError`](https://msdn.microsoft.com/library/system.web.httpserverutility.getlasterror.aspx) del objeto de servidor para recuperar los detalles de la excepción no controlada que hizo que se activara el evento `Error`.

[!code-csharp[Main](processing-unhandled-exceptions-cs/samples/sample2.cs)]

El método `GetLastError` devuelve un objeto de tipo `Exception`, que es el tipo base para todas las excepciones de la .NET Framework. Sin embargo, en el código anterior, convirtiendo el objeto de excepción devuelto por `GetLastError` en un objeto `HttpException`. Si se desencadena el evento `Error` porque se produjo una excepción durante el procesamiento de un recurso ASP.NET, la excepción que se produjo se ajusta dentro de un `HttpException`. Para obtener la excepción real que motivó el evento de error, use la propiedad `InnerException`. Si el evento de `Error` se generó debido a una excepción basada en HTTP, como una solicitud de una página no existente, se produce una `HttpException`, pero no tiene una excepción interna.

En el código siguiente se usa el `GetLastErrormessage` para recuperar información sobre la excepción que desencadenó el evento de `Error`, almacenando el `HttpException` en una variable denominada `lastErrorWrapper`. A continuación, almacena el tipo, el mensaje y el seguimiento de la pila de la excepción de origen en tres variables de cadena, comprobando si el `lastErrorWrapper` es la excepción real que desencadenó el evento de `Error` (en el caso de excepciones basadas en HTTP) o si es simplemente un contenedor para una excepción que se produjo al procesar la solicitud.

[!code-csharp[Main](processing-unhandled-exceptions-cs/samples/sample3.cs)]

Llegados a este punto, tendrá toda la información que necesita para escribir código que registrará los detalles de la excepción en una tabla de base de datos. Puede crear una tabla de base de datos con columnas para cada uno de los detalles de error de interés: el tipo, el mensaje, el seguimiento de la pila, etc., junto con otros datos útiles, como la dirección URL de la página solicitada y el nombre del usuario que ha iniciado sesión actualmente. En el controlador de eventos `Application_Error`, se debe conectar a la base de datos e insertar un registro en la tabla. Del mismo modo, puede agregar código para avisar a un desarrollador del error por correo electrónico.

Las bibliotecas de registro de errores examinadas en los dos tutoriales siguientes ofrecen esta funcionalidad de forma integrada, por lo que no hay necesidad de crear este registro de errores y la notificación. Sin embargo, para ilustrar que se genera el evento `Error` y que se puede usar el controlador de eventos `Application_Error` para registrar los detalles del error y notificar a un desarrollador, vamos a agregar código que notifique a un desarrollador cuando se produce un error.

## <a name="notifying-a-developer-when-an-unhandled-exception-occurs"></a>Notificar a un desarrollador cuando se produce una excepción no controlada

Cuando se produce una excepción no controlada en el entorno de producción, es importante avisar al equipo de desarrollo para que pueda evaluar el error y determinar las acciones que se deben realizar. Por ejemplo, si se produce un error al conectarse a la base de datos, tendrá que comprobar la cadena de conexión y, quizás, abrir una incidencia de soporte técnico con la empresa de hospedaje Web. Si la excepción se produjo debido a un error de programación, puede que sea necesario agregar código adicional o lógica de validación para evitar estos errores en el futuro.

Las clases .NET Framework del [espacio de nombres`System.Net.Mail`](https://msdn.microsoft.com/library/system.net.mail.aspx) facilitan el envío de un correo electrónico. La [clase`MailMessage`](https://msdn.microsoft.com/library/system.net.mail.mailmessage.aspx) representa un mensaje de correo electrónico y tiene propiedades como `To`, `From`, `Subject`, `Body`y `Attachments`. El `SmtpClass` se utiliza para enviar un objeto `MailMessage` mediante un servidor SMTP especificado. la configuración del servidor SMTP se puede especificar mediante programación o mediante declaración en el [elemento`<system.net>`](https://msdn.microsoft.com/library/6484zdc1.aspx) del `Web.config file`. Para obtener más información sobre el envío de mensajes de correo electrónico en una aplicación de ASP.NET, consulte el artículo sobre el [envío de correo electrónico en ASP.net](http://aspnet.4guysfromrolla.com/articles/072606-1.aspx)y las [preguntas más frecuentes del sistema .net. mail](http://systemnetmail.com/).

> [!NOTE]
> El elemento `<system.net>` contiene la configuración del servidor SMTP utilizada por la clase `SmtpClient` al enviar un correo electrónico. Probablemente, su empresa de hospedaje web tiene un servidor SMTP que puede usar para enviar correo electrónico desde su aplicación. Consulte la sección de soporte técnico del host web para obtener información sobre la configuración del servidor SMTP que debe usar en la aplicación Web.

Agregue el código siguiente al controlador de eventos `Application_Error` para enviar un correo electrónico a un desarrollador cuando se produzca un error:

[!code-csharp[Main](processing-unhandled-exceptions-cs/samples/sample4.cs)]

Aunque el código anterior es bastante largo, la mayoría de ellos crea el código HTML que aparece en el correo electrónico que se envía al desarrollador. El código comienza haciendo referencia al `HttpException` devuelto por el método `GetLastError` (`lastErrorWrapper`). La excepción real generada por la solicitud se recupera a través de `lastErrorWrapper.InnerException` y se asigna a la variable `lastError`. El tipo, el mensaje y la información de seguimiento de la pila se recuperan de `lastError` y se almacenan en tres variables de cadena.

A continuación, se crea un objeto de `MailMessage` denominado `mm`. El cuerpo del correo electrónico está en formato HTML y muestra la dirección URL de la página solicitada, el nombre del usuario que ha iniciado la sesión actual e información sobre la excepción (el tipo, el mensaje y el seguimiento de la pila). Una de las cosas interesantes sobre la clase `HttpException` es que puede generar el código HTML que se usa para crear la pantalla de detalles de la excepción en amarillo (YSOD) mediante una llamada al [método GetHtmlErrorMessage](https://msdn.microsoft.com/library/system.web.httpexception.gethtmlerrormessage.aspx). Este método se usa aquí para recuperar el marcado YSOD de detalles de la excepción y agregarlo al correo electrónico como datos adjuntos. Una palabra de PRECAUCIÓN: Si la excepción que desencadenó el evento `Error` era una excepción basada en HTTP (como una solicitud de una página que no existe), el método `GetHtmlErrorMessage` devolverá `null`.

El último paso es enviar el `MailMessage`. Esto se hace creando un nuevo método de `SmtpClient` y llamando a su método `Send`.

> [!NOTE]
> Antes de usar este código en la aplicación Web, querrá cambiar los valores de las constantes `ToAddress` y `FromAddress` de support@example.com a cualquier dirección de correo electrónico a la que se debe enviar el correo electrónico de notificación de errores y desde la que se origina. También deberá especificar la configuración del servidor SMTP en la sección `<system.net>` en `Web.config`. Consulte al proveedor de host web para determinar la configuración del servidor SMTP que se va a usar.

Con este código en su lugar, siempre que haya un error, se envía un mensaje de correo electrónico al desarrollador que resume el error e incluye el YSOD. En el tutorial anterior, mostramos un error en tiempo de ejecución visitando Genre. aspx y pasando un valor de `ID` no válido a través de la cadena de consulta, como `Genre.aspx?ID=foo`. Al visitar la página con el archivo de `Global.asax` en su lugar, se obtiene la misma experiencia de usuario que en el tutorial anterior: en el entorno de desarrollo, seguirá viendo la pantalla de detalles de la excepción en amarillo, mientras que en el entorno de producción verá la página de error personalizada. Además de este comportamiento existente, se envía un correo electrónico al desarrollador.

En la **ilustración 2** se muestra el correo electrónico recibido al visitar `Genre.aspx?ID=foo`. El cuerpo del correo electrónico resume la información de la excepción, mientras que el `YSOD.htm` datos adjuntos muestra el contenido que se muestra en los detalles de la excepción YSOD (vea la **figura 3**).

[![](processing-unhandled-exceptions-cs/_static/image5.png)](processing-unhandled-exceptions-cs/_static/image4.png)

**Figura 2**: se envía una notificación por correo electrónico al desarrollador cada vez que hay una excepción no controlada  
([Haga clic para ver la imagen de tamaño completo](processing-unhandled-exceptions-cs/_static/image6.png))

[![](processing-unhandled-exceptions-cs/_static/image8.png)](processing-unhandled-exceptions-cs/_static/image7.png)

**Figura 3**: la notificación de correo electrónico incluye los detalles de la excepción YSOD como datos adjuntos  
([Haga clic para ver la imagen de tamaño completo](processing-unhandled-exceptions-cs/_static/image9.png))

## <a name="what-about-using-the-custom-error-page"></a>¿Cómo se usa la página de error personalizada?

En este tutorial se ha mostrado cómo usar `Global.asax` y el controlador de eventos `Application_Error` para ejecutar código cuando se produce una excepción no controlada. En concreto, usamos este controlador de eventos para notificar a un desarrollador de un error. podríamos ampliarla para registrar también los detalles del error en una base de datos. La presencia del controlador de eventos `Application_Error` no afecta a la experiencia del usuario final. Todavía ven la página de error configurada, es decir, los detalles del error YSOD, el error de tiempo de ejecución YSOD o la página de error personalizada.

Es natural preguntarse si el archivo de `Global.asax` y el evento de `Application_Error` son necesarios cuando se usa una página de error personalizada. Cuando se produce un error, se muestra al usuario la página de error personalizada, por lo que no se puede incluir el código para notificar al desarrollador y registrar los detalles del error en la clase de código subyacente de la página de error personalizada. Aunque puede agregar código a la clase de código subyacente de la página de error personalizada, no tiene acceso a los detalles de la excepción que desencadenó el evento `Error` cuando se usa la técnica que se ha explorado en el tutorial anterior. La llamada al método `GetLastError` desde la página de error personalizada devuelve `Nothing`.

La razón de este comportamiento se debe a que la página de error personalizada se alcanza a través de una redirección. Cuando una excepción no controlada alcanza el tiempo de ejecución de ASP.NET, el motor de ASP.NET genera su evento `Error` (que ejecuta el controlador de eventos `Application_Error`) y, a continuación, *redirige* al usuario a la página de error personalizada emitiendo una `Response.Redirect(customErrorPageUrl)`. El método `Response.Redirect` envía una respuesta al cliente con un código de Estado HTTP 302, que indica al explorador que solicite una nueva dirección URL, es decir, la página de error personalizada. A continuación, el explorador solicita automáticamente esta nueva página. Puede indicar que la página de error personalizada se solicitó por separado de la página en la que se originó el error porque la barra de direcciones del explorador cambia a la dirección URL de la página de errores personalizada (consulte la **figura 4**).

[![](processing-unhandled-exceptions-cs/_static/image11.png)](processing-unhandled-exceptions-cs/_static/image10.png)

**Figura 4**: cuando se produce un error, el explorador se redirige a la dirección URL de la página de errores personalizada  
([Haga clic para ver la imagen de tamaño completo](processing-unhandled-exceptions-cs/_static/image12.png))

El efecto neto es que la solicitud en la que se produjo la excepción no controlada finaliza cuando el servidor responde con la redirección HTTP 302. La solicitud posterior a la página de error personalizada es una solicitud nueva de marca; en este momento, el motor de ASP.NET ha descartado la información de error y, además, no tiene ninguna manera de asociar la excepción no controlada en la solicitud anterior con la nueva solicitud de la página de error personalizada. Este es el motivo por el que `GetLastError` devuelve `null` cuando se llama desde la página de error personalizada.

Sin embargo, es posible que se ejecute la página de error personalizada durante la misma solicitud que causó el error. El método [`Server.Transfer(url)`](https://msdn.microsoft.com/library/system.web.httpserverutility.transfer.aspx) transfiere la ejecución a la dirección URL especificada y la procesa dentro de la misma solicitud. Puede trasladar el código del controlador de eventos `Application_Error` a la clase de código subyacente de la página de error personalizada, reemplazándolo en `Global.asax` con el siguiente código:

[!code-csharp[Main](processing-unhandled-exceptions-cs/samples/sample5.cs)]

Ahora, cuando se produce una excepción no controlada, el controlador de eventos `Application_Error` transfiere el control a la página de error personalizada correspondiente según el código de Estado HTTP. Dado que se ha transferido el control, la página de error personalizada tiene acceso a la información de excepción no controlada a través de `Server.GetLastError` y puede notificar al desarrollador el error y registrar sus detalles. La llamada a `Server.Transfer` detiene el motor ASP.NET para redirigir al usuario a la página de error personalizada. En su lugar, se devuelve el contenido de la página de error personalizada como respuesta a la página que generó el error.

## <a name="summary"></a>Resumen

Cuando se produce una excepción no controlada en una aplicación Web de ASP.NET, el tiempo de ejecución de ASP.NET genera el evento de `Error` y muestra la página de error configurada. Podemos notificar al desarrollador el error, registrar sus detalles o procesarlo de otra manera, creando un controlador de eventos para el evento de error. Hay dos maneras de crear un controlador de eventos para `HttpApplication` eventos como `Error`: en el archivo `Global.asax` o en un módulo HTTP. En este tutorial se ha mostrado cómo crear un controlador de eventos `Error` en el archivo `Global.asax` que notifica a los desarrolladores de un error mediante un mensaje de correo electrónico.

Crear un controlador de eventos `Error` es útil si necesita procesar las excepciones no controladas de alguna manera única o personalizada. Sin embargo, la creación de su propio `Error` controlador de eventos para registrar la excepción o para notificar a un desarrollador no es el uso más eficaz de su tiempo, ya que ya existen bibliotecas de registro de errores gratuitas y fáciles de usar que se pueden configurar en cuestión de minutos. En los dos tutoriales siguientes se examinan dos bibliotecas de este tipo.

¡ Feliz programación!

### <a name="further-reading"></a>Información adicional

Para obtener más información sobre los temas descritos en este tutorial, consulte los siguientes recursos:

- [Información general de los módulos http y los controladores HTTP ASP.NET](https://support.microsoft.com/kb/307985)
- [Responder correctamente a las excepciones no controladas: procesamiento de excepciones no controladas](http://aspnet.4guysfromrolla.com/articles/091306-1.aspx)
- [`HttpApplication` clase y el objeto de aplicación ASP.NET](http://www.eggheadcafe.com/articles/20030211.asp)
- [Controladores HTTP y módulos HTTP en ASP.NET](http://www.15seconds.com/Issue/020417.htm)
- [Envío de correo electrónico en ASP.NET](http://aspnet.4guysfromrolla.com/articles/072606-1.aspx)
- [Descripción del archivo de `Global.asax`](http://aspalliance.com/1114_Understanding_the_Globalasax_file.all)
- [Uso de módulos y controladores HTTP para crear componentes ASP.NET conectables](https://msdn.microsoft.com/library/aa479332.aspx)
- [Trabajar con el archivo de `Global.asax` de ASP.NET](http://articles.techrepublic.com.com/5100-10878_11-5771721.html)
- [Trabajar con instancias de `HttpApplication`](https://msdn.microsoft.com/library/a0xez8f2.aspx)

> [!div class="step-by-step"]
> [Anterior](displaying-a-custom-error-page-cs.md)
> [Siguiente](logging-error-details-with-asp-net-health-monitoring-cs.md)
