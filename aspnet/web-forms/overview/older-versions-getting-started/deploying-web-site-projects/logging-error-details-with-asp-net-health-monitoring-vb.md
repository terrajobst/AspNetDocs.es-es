---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/logging-error-details-with-asp-net-health-monitoring-vb
title: Registrar detalles de error con la supervisión de estado de ASP.NET (VB) | Microsoft Docs
author: rick-anderson
description: El sistema de supervisión de estado de Microsoft proporciona una manera sencilla y personalizable de registrar varios eventos Web, incluidas las excepciones no controladas. En este tutorial se recorre mismos...
ms.author: riande
ms.date: 06/09/2009
ms.assetid: 09a6c74e-936a-4c04-8547-5bb313a4e4a3
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/logging-error-details-with-asp-net-health-monitoring-vb
msc.type: authoredcontent
ms.openlocfilehash: f57aca41771adfd9a7c7f38da1916db9197262da
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/28/2019
ms.locfileid: "74587830"
---
# <a name="logging-error-details-with-aspnet-health-monitoring-vb"></a>Registrar detalles de error con Supervisión de estado de ASP.NET (VB)

por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descargar código](https://download.microsoft.com/download/1/0/C/10CC829F-A808-4302-97D3-59989B8F9C01/ASPNET_Hosting_Tutorial_13_VB.zip) o [Descargar PDF](https://download.microsoft.com/download/5/C/5/5C57DB8C-5DEA-4B3A-92CA-4405544D313B/aspnet_tutorial13_HealthMonitoring_vb.pdf)

> El sistema de supervisión de estado de Microsoft proporciona una manera sencilla y personalizable de registrar varios eventos Web, incluidas las excepciones no controladas. Este tutorial le guía a través de la configuración del sistema de supervisión de estado para registrar excepciones no controladas en una base de datos y para notificar a los desarrolladores a través de un mensaje de correo electrónico.

## <a name="introduction"></a>Introducción

El registro es una herramienta útil para supervisar el estado de una aplicación implementada y para diagnosticar los problemas que puedan surgir. Es especialmente importante registrar los errores que se producen en una aplicación implementada para que se puedan solucionar. El evento `Error` se desencadena cuando se produce una excepción no controlada en una aplicación ASP.NET; en el [tutorial anterior](processing-unhandled-exceptions-vb.md) se mostró cómo notificar a un desarrollador de un error y registrar sus detalles mediante la creación de un controlador de eventos para el evento `Error`. Sin embargo, la creación de un controlador de eventos `Error` para registrar los detalles del error y notificar a un desarrollador no es necesario, ya que esta tarea la puede realizar ASP. Sistema de *supervisión de estado*de la red.

El sistema de seguimiento de estado se presentó en ASP.NET 2,0 y está diseñado para supervisar el estado de una aplicación ASP.NET implementada mediante el registro de los eventos que se producen durante la vigencia de la aplicación o la solicitud. Los eventos registrados por el sistema de supervisión de mantenimiento se conocen como *eventos de supervisión de estado* o *eventos Web*e incluyen:

- Eventos de duración de la aplicación, como cuando se inicia o se detiene una aplicación
- Eventos de seguridad, incluidos los intentos de inicio de sesión erróneos y las solicitudes de autorización de URL erróneas
- Los errores de la aplicación, incluidas las excepciones no controladas, las excepciones de análisis del estado de la vista, las excepciones de validación de solicitudes y los errores de compilación, entre otros tipos de errores.

Cuando se produce un evento de supervisión de estado, se puede registrar en cualquier número de *orígenes de registro*especificados. El sistema de seguimiento de estado se suministra con orígenes de registro que registran eventos Web en una base de datos de Microsoft SQL Server, en el registro de eventos de Windows o a través de un mensaje de correo electrónico, entre otros. También puede crear sus propios orígenes de registro.

Los eventos que los registros del sistema de supervisión de estado, junto con los orígenes de registro utilizados, se definen en `Web.config`. Con unas pocas líneas de marcado de configuración puede usar la supervisión de estado para registrar todas las excepciones no controladas en una base de datos y para notificarle la excepción por correo electrónico.

## <a name="exploring-the-health-monitoring-systems-configuration"></a>Exploración de la configuración del sistema de supervisión de estado

El comportamiento del sistema de supervisión de estado se define mediante su información de configuración, que se encuentra en el [elemento`<healthMonitoring>`](https://msdn.microsoft.com/library/2fwh2ss9.aspx) en `Web.config`. Esta sección de configuración define, entre otras cosas, los tres elementos de información importantes siguientes:

1. Los eventos de supervisión de estado que, cuando se producen, deben registrarse,
2. Los orígenes de registro y
3. Cómo se asignan todos los eventos de supervisión de estado definidos en (1) a los orígenes de registro definidos en (2).

Esta información se especifica mediante tres elementos de configuración secundarios: [`<eventMappings>`](https://msdn.microsoft.com/library/yc5yk01w.aspx), [`<providers>`](https://msdn.microsoft.com/library/zaa41kz1.aspx)y [`<rules>`](https://msdn.microsoft.com/library/fe5wyxa0.aspx), respectivamente.

La información de configuración del sistema de supervisión de estado predeterminada se puede encontrar en el archivo `Web.config` en `%WINDIR%\Microsoft.NET\Framework\version\CONFIG` carpeta. Esta información de configuración predeterminada, con algún marcado quitado por brevedad, se muestra a continuación:

[!code-xml[Main](logging-error-details-with-asp-net-health-monitoring-vb/samples/sample1.xml)]

Los eventos de seguimiento de estado de interés se definen en el elemento `<eventMappings>`, que proporciona un nombre descriptivo a una clase de eventos de supervisión de estado. En el marcado anterior, el elemento `<eventMappings>` asigna el nombre descriptivo "todos los errores" a los eventos de supervisión de estado de tipo `WebBaseErrorEvent` y el nombre "auditorías de error" a los eventos de supervisión de estado de tipo `WebFailureAuditEvent`.

El elemento `<providers>` define los orígenes de registro y les proporciona un nombre descriptivo y especifica cualquier información de configuración específica del origen del registro. El primer elemento `<add>` define el proveedor "EventLogProvider", que registra los eventos de supervisión de estado especificados mediante la clase `EventLogWebEventProvider`. La clase `EventLogWebEventProvider` registra el evento en el registro de eventos de Windows. El segundo elemento `<add>` define el proveedor "SqlWebEventProvider", que registra los eventos en una base de datos Microsoft SQL Server a través de la clase `SqlWebEventProvider`. La configuración de "SqlWebEventProvider" especifica la cadena de conexión de la base de datos (`connectionStringName`) entre otras opciones de configuración.

El elemento `<rules>` asigna los eventos especificados en el elemento `<eventMappings>` a los orígenes de registro del elemento `<providers>`. De forma predeterminada, las aplicaciones Web de ASP.NET registran todas las excepciones no controladas y los errores de auditoría en el registro de eventos de Windows.

## <a name="logging-events-to-a-database"></a>Registrar eventos en una base de datos

La configuración predeterminada del sistema de seguimiento de estado se puede personalizar en una aplicación Web de aplicación por Web agregando una sección `<healthMonitoring>` al archivo de `Web.config` de la aplicación. Puede incluir elementos adicionales en las secciones `<eventMappings>`, `<providers>`y `<rules>` mediante el elemento `<add>`. Para quitar una configuración de la configuración predeterminada, use el elemento `<remove>`, o use `<clear />` para quitar todos los valores predeterminados de una de estas secciones. Vamos a configurar la aplicación Web Book reviews para registrar todas las excepciones no controladas en una base de datos de Microsoft SQL Server mediante la clase `SqlWebEventProvider`.

La clase `SqlWebEventProvider` forma parte del sistema de supervisión de estado y registra un evento de supervisión de estado en una base de datos de SQL Server especificada. La clase `SqlWebEventProvider` espera que la base de datos especificada incluya un procedimiento almacenado denominado `aspnet_WebEvent_LogEvent`. A este procedimiento almacenado se le pasan los detalles del evento y se les aplica el almacenamiento de los detalles del evento. La buena noticia es que no es necesario crear este procedimiento almacenado ni la tabla para almacenar los detalles del evento. Puede agregar estos objetos a la base de datos mediante la herramienta `aspnet_regsql.exe`.

> [!NOTE]
> La herramienta de `aspnet_regsql.exe` se analizó en el [tutorial *configuración de un sitio web que usa servicios de aplicación* ](configuring-a-website-that-uses-application-services-vb.md) cuando agregamos compatibilidad con ASP. Servicios de aplicaciones de la red. Por lo tanto, la base de datos del sitio web de reseñas del libro ya contiene el `aspnet_WebEvent_LogEvent` procedimiento almacenado, que almacena la información del evento en una tabla denominada `aspnet_WebEvent_Events`.

Una vez que tenga el procedimiento almacenado y la tabla necesarios agregados a la base de datos, todo lo que queda es indicar a la supervisión de estado que registre todas las excepciones no controladas en la base de datos. Para ello, agregue el marcado siguiente al archivo `Web.config` del sitio web:

[!code-xml[Main](logging-error-details-with-asp-net-health-monitoring-vb/samples/sample2.xml)]

El marcado de configuración de supervisión de estado anterior usa elementos `<clear />` para borrar la información de configuración de supervisión de estado predefinida de las secciones `<eventMappings>`, `<providers>`y `<rules>`. A continuación, agrega una entrada única a cada una de estas secciones.

- El elemento `<eventMappings>` define un único evento de supervisión de estado de interés denominado "All Errors", que se genera cada vez que se produce una excepción no controlada.
- El elemento `<providers>` define un solo origen de registro denominado "SqlWebEventProvider" que usa la clase `SqlWebEventProvider`. El atributo `connectionStringName` se ha establecido en "ReviewsConnectionString", que es el nombre de la cadena de conexión definida en la sección `<connectionStrings>`.
- Por último, el elemento &lt;rules&gt; indica que, cuando se produce un evento "All Errors", se debe registrar con el proveedor "SqlWebEventProvider".

Esta información de configuración indica al sistema de supervisión de estado que registre todas las excepciones no controladas en la base de datos de revisiones del libro.

> [!NOTE]
> El evento `WebBaseErrorEvent` solo se produce para los errores del servidor; no se produce para los errores HTTP, como una solicitud para un recurso ASP.NET que no se encuentra. Esto difiere del comportamiento del evento `Error` de la clase `HttpApplication`, que se produce para los errores de servidor y HTTP.

Para ver el sistema de supervisión de estado en acción, visite el sitio web y genere un error en tiempo de ejecución visitando `Genre.aspx?ID=foo`. Debería ver la página de error adecuada, ya sea la pantalla de detalles de la excepción en amarillo (al visitar localmente) o la página de error personalizada (al visitar el sitio en producción). En segundo plano, el sistema de supervisión de estado registró la información de error en la base de datos. Debe haber un registro en la tabla `aspnet_WebEvent_Events` (vea la **Ilustración 1**). Este registro contiene información sobre el error de tiempo de ejecución que se acaba de producir.

[![](logging-error-details-with-asp-net-health-monitoring-vb/_static/image2.png)](logging-error-details-with-asp-net-health-monitoring-vb/_static/image1.png)

**Figura 1**: los detalles del error se registraron en la tabla `aspnet_WebEvent_Events`  
([Haga clic para ver la imagen de tamaño completo](logging-error-details-with-asp-net-health-monitoring-vb/_static/image3.png))

### <a name="displaying-the-error-log-in-a-web-page"></a>Mostrar el registro de errores en una página web

Con la configuración actual del sitio web, el sistema de supervisión de estado registra todas las excepciones no controladas en la base de datos. Sin embargo, la supervisión de estado no proporciona ningún mecanismo para ver el registro de errores a través de una página web. Sin embargo, podría crear una página ASP.NET que muestre esta información de la base de datos. (Como veremos en breve, puede optar por que se le envíen los detalles del error en un mensaje de correo electrónico).

Si crea este tipo de página, asegúrese de seguir los pasos necesarios para permitir que solo los usuarios autorizados vean los detalles del error. Si el sitio ya emplea cuentas de usuario, puede usar reglas de autorización de URL para restringir el acceso a la página a determinados usuarios o roles. Para obtener más información sobre cómo conceder o restringir el acceso a las páginas web en función del usuario que ha iniciado sesión, consulte los [tutoriales de seguridad de los sitios web](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md).

> [!NOTE]
> En el tutorial siguiente se explora un sistema de notificación y un registro de errores alternativos denominado ELMAH. ELMAH incluye un mecanismo integrado para ver el registro de errores de una página web y como una fuente RSS.

## <a name="logging-events-to-email"></a>Registrar eventos en el correo electrónico

El sistema de supervisión de estado incluye un proveedor de origen de registro que "registra" un evento en un mensaje de correo electrónico. El origen de registro incluye la misma información que se registra en la base de datos en el cuerpo del mensaje de correo electrónico. Puede usar este origen de registro para notificar a un desarrollador cuando se produce un determinado evento de supervisión de estado.

Vamos a actualizar la configuración del sitio web de revisiones del libro para recibir un correo electrónico cada vez que se produzca una excepción. Para lograr esto, es necesario realizar tres tareas:

1. Configure la aplicación Web ASP.NET para que envíe correo electrónico. Esto se logra especificando cómo se envían los mensajes de correo electrónico a través del elemento de configuración `<system.net>`. Para obtener más información sobre el envío de mensajes de correo electrónico en una aplicación ASP.NET, consulte [envío de correo electrónico en ASP.net](http://aspnet.4guysfromrolla.com/articles/072606-1.aspx) y [preguntas más frecuentes del sistema .net. mail](http://systemnetmail.com/).
2. Registre el proveedor de origen del registro de correo electrónico en el elemento `<providers>`, y
3. Agregue una entrada al elemento `<rules>` que asigna el evento "All Errors" al proveedor de origen de registro agregado en el paso (2).

El sistema de supervisión de estado incluye dos clases de proveedor de origen de registro de correo electrónico: `SimpleMailWebEventProvider` y `TemplatedMailWebEventProvider`. La [clase`SimpleMailWebEventProvider`](https://msdn.microsoft.com/library/system.web.management.simplemailwebeventprovider.aspx) envía un mensaje de correo electrónico de texto sin formato que incluye los detalles del evento y proporciona poca personalización del cuerpo del correo electrónico. Con la [clase`TemplatedMailWebEventProvider`](https://msdn.microsoft.com/library/system.web.management.templatedmailwebeventprovider.aspx) se especifica una página ASP.net cuyo marcado representado se usa como cuerpo del mensaje de correo electrónico. La [clase`TemplatedMailWebEventProvider`](https://msdn.microsoft.com/library/system.web.management.templatedmailwebeventprovider.aspx) proporciona un mayor control sobre el contenido y el formato del mensaje de correo electrónico, pero requiere un poco más trabajo inicial, ya que tiene que crear la página ASP.net que genera el cuerpo del mensaje de correo electrónico. Este tutorial se centra en el uso de la clase `SimpleMailWebEventProvider`.

Actualice el elemento `<providers>` del sistema de supervisión de estado en el archivo `Web.config` para incluir un origen de registro para la clase `SimpleMailWebEventProvider`:

[!code-xml[Main](logging-error-details-with-asp-net-health-monitoring-vb/samples/sample3.xml)]

El marcado anterior utiliza la clase `SimpleMailWebEventProvider` como proveedor de origen de registro y le asigna el nombre descriptivo "EmailWebEventProvider". Además, el atributo `<add>` incluye opciones de configuración adicionales, como las direcciones to y from del mensaje de correo electrónico.

Con el origen de registro de correo electrónico definido, todo lo que queda es indicar al sistema de supervisión de estado que use este origen para "registrar" las excepciones no controladas. Esto se logra agregando una nueva regla en la sección `<rules>`:

[!code-xml[Main](logging-error-details-with-asp-net-health-monitoring-vb/samples/sample4.xml)]

La sección `<rules>` incluye ahora dos reglas. La primera, denominada "todos los errores de correo electrónico", envía todas las excepciones no controladas al origen de registro "EmailWebEventProvider". Esta regla tiene el efecto de enviar detalles sobre los errores en el sitio web a la dirección especificada. La regla "todos los errores a la base de datos" registra los detalles del error en la base de datos del sitio. Por lo tanto, siempre que se produzca una excepción no controlada en el sitio, sus detalles se registrarán en la base de datos y se enviarán a la dirección de correo electrónico especificada.

En la **ilustración 2** se muestra el correo electrónico generado por la clase `SimpleMailWebEventProvider` al visitar `Genre.aspx?ID=foo`.

[![](logging-error-details-with-asp-net-health-monitoring-vb/_static/image5.png)](logging-error-details-with-asp-net-health-monitoring-vb/_static/image4.png)

**Figura 2**: los detalles del error se envían en un mensaje de correo electrónico  
([Haga clic para ver la imagen de tamaño completo](logging-error-details-with-asp-net-health-monitoring-vb/_static/image6.png))

## <a name="summary"></a>Resumen

El sistema de supervisión de estado de ASP.NET está diseñado para permitir a los administradores supervisar el estado de una aplicación web implementada. Los eventos de supervisión de estado se generan cuando se desconectan ciertas acciones, como cuando la aplicación se detiene, cuando un usuario inicia sesión correctamente en el sitio o cuando se produce una excepción no controlada. Estos eventos se pueden registrar en cualquier número de orígenes de registro. En este tutorial se ha mostrado cómo registrar los detalles de las excepciones no controladas en una base de datos y a través de un mensaje de correo electrónico.

En este tutorial se ha centrado en el uso de la supervisión de estado para registrar excepciones no controladas, pero tenga en cuenta que la supervisión de estado está diseñada para medir el estado general de una aplicación de ASP.NET implementada e incluye una gran cantidad de eventos de supervisión de estado y orígenes de registro no explorado aquí. Además, puede crear sus propios eventos de supervisión de estado y orígenes de registro, en caso de que sea necesario. Si está interesado en obtener más información sobre la supervisión de estado, un buen paso es leer las [preguntas más frecuentes](https://blogs.msdn.com/erikreitan/archive/2006/05/22/603586.aspx)sobre la supervisión de estado de [Erik Reitan](https://blogs.msdn.com/erikreitan/archive/2006/05/22/603586.aspx). A continuación, consulte [Cómo: usar el seguimiento de estado en ASP.NET 2,0](https://msdn.microsoft.com/library/ms998306.aspx).

¡ Feliz programación!

### <a name="further-reading"></a>Información adicional

Para obtener más información sobre los temas descritos en este tutorial, consulte los siguientes recursos:

- [Información general de supervisión de estado de ASP.NET](https://msdn.microsoft.com/library/bb398933.aspx)
- [Configuración y personalización del sistema de supervisión de estado de ASP.NET](http://dotnetslackers.com/articles/aspnet/ConfiguringAndCustomizingTheHealthMonitoringSystemOfASPNET.aspx)
- [Preguntas más frecuentes: seguimiento de estado en ASP.NET 2,0](https://blogs.msdn.com/erikreitan/archive/2006/05/22/603586.aspx)
- [Cómo: enviar correo electrónico para notificaciones de supervisión de estado](https://msdn.microsoft.com/library/ms227553.aspx)
- [Cómo: usar la supervisión de estado en ASP.NET](https://msdn.microsoft.com/library/ms998306.aspx)
- [Seguimiento de estado en ASP.NET](http://aspnet.4guysfromrolla.com/articles/031407-1.aspx)

> [!div class="step-by-step"]
> [Anterior](processing-unhandled-exceptions-vb.md)
> [Siguiente](logging-error-details-with-elmah-vb.md)
