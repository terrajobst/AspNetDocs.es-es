---
uid: aspnet/overview/web-development-best-practices/what-not-to-do-in-aspnet-and-what-to-do-instead
title: Qué no hacer en ASP.NET y qué hacer en su lugar | Microsoft Docs
author: Rick-Anderson
description: En este tema se describen varios errores comunes que los usuarios realizan en los proyectos Web de ASP.NET. Proporciona recomendaciones sobre lo que debe hacer para evitar estas comunicaciones...
ms.author: riande
ms.date: 01/28/2019
ms.assetid: c39b9965-545c-4b04-8f55-21be7f28a9e5
msc.legacyurl: /aspnet/overview/web-development-best-practices/what-not-to-do-in-aspnet-and-what-to-do-instead
msc.type: authoredcontent
ms.openlocfilehash: 980d3544df70643043391e6573803ce21b3a824f
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78500137"
---
# <a name="what-not-to-do-in-aspnet-and-what-to-do-instead"></a>Qué no se debe hacer en ASP.NET y qué hacer en su lugar

> En este tema se describen varios errores comunes que los usuarios realizan en los proyectos Web de ASP.NET. Proporciona recomendaciones sobre lo que debe hacer para evitar estos errores comunes. Se basa en una [presentación](http://vimeo.com/68390507) de **Damian Edwards** en la Conferencia de desarrolladores de noruego.

## <a name="disclaimer"></a>Declinación de responsabilidades

Este tema no pretende ser una guía completa para asegurarse de que la aplicación sea segura y eficaz. Todavía debe seguir los procedimientos recomendados de seguridad y rendimiento que no se describen en este tema. Solo sugiere cómo evitar errores comunes relacionados con las clases y los procesos de .NET.

## <a name="overview"></a>Información general

Este tema contiene las siguientes secciones:

- [Cumplimiento de estándares](#standards)

    - [Adaptadores de control](#adapters)
    - [Propiedades de estilo en controles](#styleprop)
    - [Devoluciones de llamada de páginas y controles](#callback)
    - [Detección de capacidad del explorador](#browsercap)
- [Seguridad](#security)

    - [Validación de solicitudes](#validation)
    - [Autenticación y sesión de formularios sin cookies](#cookieless)
    - [EnableViewStateMac](#viewstatemac)
    - [Confianza media](#medium)
    - [&lt;appSettings&gt;](#appsettings)
    - [UrlPathEncode](#urlpathencode)
- [Confiabilidad y rendimiento](#performance)

    - [PreSendRequestHeaders y PreSendRequestContent](#presend)
    - [Eventos de página asincrónica con formularios Web Forms](#asyncevents)
    - [Trabajo de incendio y olvidar](#fire)
    - [Cuerpo de la entidad de solicitud](#requestentity)
    - [Response. Redirect y Response. end](#redirect)
    - [EnableViewState y ViewStateMode](#viewstatemode)
    - [SqlMembershipProvider](#sqlprovider)
    - [Solicitudes de ejecución prolongada (> 110 segundos)](#long)

<a id="standards"></a>

## <a name="standards-compliance"></a>Cumplimiento de estándares

<a id="adapters"></a>

### <a name="control-adapters"></a>Adaptadores de control

Recomendación: dejar de usar adaptadores de control para la representación adaptable y, en su lugar, usar las consultas multimedia de CSS y HTML compatible con los estándares.

Los adaptadores de controles se introdujeron en .NET 2,0 para representar el código de presentación que se personalizó para diferentes dispositivos y entornos. Ahora, esta representación adaptable se puede realizar con CSS y HTML. Debe dejar de usar los adaptadores de control y convertir los adaptadores existentes en CSS y HTML.

Para obtener más información, vea [consultas multimedia](http://www.w3.org/TR/css3-mediaqueries/) y [Cómo: agregar páginas móviles a la aplicación web FORMs/MVC de ASP.net](../../../whitepapers/add-mobile-pages-to-your-aspnet-web-forms-mvc-application.md).

<a id="styleprop"></a>

### <a name="style-properties-on-controls"></a>Propiedades de estilo en controles

Recomendación: detenga el establecimiento de valores de estilo en el marcado de control y, en su lugar, establezca valores de formato en hojas de estilos CSS.

Los controles de servidor web contienen docenas de propiedades que se pueden usar para establecer las propiedades de estilo en línea. Por ejemplo, la propiedad ForeColor establece el color del texto de un control. Puede lograr este mismo efecto más eficazmente a través de hojas de estilo CSS. Las hojas de estilos permiten centralizar los valores de estilo y evitar establecer estos valores en toda la aplicación.

En el ejemplo siguiente se muestra una clase CSS que establece el texto en rojo.

[!code-css[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample1.css)]

En el ejemplo siguiente se muestra cómo aplicar dinámicamente la clase CSS.

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample2.cs)]

<a id="callback"></a>

### <a name="page-and-control-callbacks"></a>Devoluciones de llamada de páginas y controles

Recomendación: deje de usar devoluciones de llamada de página y de control y, en su lugar, use cualquiera de los siguientes: AJAX, UpdatePanel, métodos de acción de MVC, API Web o Signalr.

En versiones anteriores de ASP.NET, los métodos de devolución de llamada de página y control permitían actualizar parte de la página web sin actualizar una página completa. Ahora puede realizar actualizaciones parciales de página a través de [Ajax](../../../ajax/index.md), [UpdatePanel](https://msdn.microsoft.com/library/bb386454.aspx), [MVC](../../../mvc/index.md), [API Web](../../../web-api/index.md) o [signalr](../../../signalr/index.md). Debe dejar de usar los métodos de devolución de llamada porque pueden causar problemas con direcciones URL y enrutamiento descriptivos. De forma predeterminada, los controles no habilitan los métodos de devolución de llamada, pero si ha habilitado esta característica en un control, debe deshabilitarla.

<a id="browsercap"></a>

### <a name="browser-capability-detection"></a>Detección de capacidad del explorador

Recomendación: deje de usar la detección de capacidad del explorador estática y, en su lugar, use la detección de características dinámicas.

En versiones anteriores de ASP.NET, las características admitidas para cada explorador se almacenaban en un archivo XML. La detección de la compatibilidad de características a través de una búsqueda estática no es el mejor enfoque. Ahora, puede detectar dinámicamente las características admitidas de un explorador mediante el uso de un marco de detección de características, como [modernizr](http://modernizr.com/). La detección de características determina la compatibilidad intentando usar un método o una propiedad y, a continuación, comprobando si el explorador generó el resultado deseado. De forma predeterminada, Modernizr se incluye en las plantillas de aplicación Web.

<a id="security"></a>

## <a name="security"></a>Seguridad

<a id="validation"></a>

### <a name="request-validation"></a>Validación de solicitudes

Recomendación: validar la entrada del usuario y codificar la salida de los usuarios.

La validación de solicitudes es una característica de ASP.NET que inspecciona cada solicitud y detiene la solicitud si se encuentra una amenaza percibida. No dependa de la validación de solicitudes para proteger su aplicación contra ataques de scripts entre sitios. En su lugar, valide toda la entrada de los usuarios y codifique la salida. En algunos casos limitados, puede usar expresiones regulares para validar la entrada, pero en casos más complicados debe validar la entrada del usuario mediante el uso de clases de .NET que determinan si el valor coincide con los valores permitidos.

En el ejemplo siguiente se muestra cómo usar un método estático en la clase URI para determinar si el URI proporcionado por un usuario es válido.

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample3.cs)]

Sin embargo, para comprobar suficientemente el URI, debe comprobar también para asegurarse de que especifica `http` o `https`. En el ejemplo siguiente se usan métodos de instancia para comprobar que el URI es válido.

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample4.cs)]

Antes de representar la entrada del usuario como HTML o incluir la entrada del usuario en una consulta SQL, codifique los valores para asegurarse de que no se incluye código malintencionado.

Puede codificar en HTML el valor en el marcado con la sintaxis &lt;%:%&gt;, como se muestra a continuación.

[!code-aspx[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample5.aspx?highlight=1)]

O bien, en sintaxis Razor, puede codificar HTML con @, como se muestra a continuación.

[!code-cshtml[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample6.cshtml?highlight=1)]

En el ejemplo siguiente se muestra cómo codificar en HTML un valor en el código subyacente.

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample7.cs)]

Para codificar un valor de forma segura para los comandos SQL, use parámetros de comando como [SqlParameter](https://msdn.microsoft.com/library/system.data.sqlclient.sqlparameter.aspx). <a id="cookieless"></a>

### <a name="cookieless-forms-authentication-and-session"></a>Autenticación y sesión de formularios sin cookies

Recomendación: requerir cookies.

No es seguro pasar la información de autenticación en la cadena de consulta. Por lo tanto, se requieren cookies cuando la aplicación incluye autenticación. Si la cookie almacena información confidencial, considere la posibilidad de requerir SSL para la cookie.

En el ejemplo siguiente se muestra cómo especificar en el archivo Web. config que la autenticación de formularios requiere una cookie que se transmite a través de SSL.

[!code-xml[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample8.xml)]

<a id="viewstatemac"></a>

### <a name="enableviewstatemac"></a>EnableViewStateMac

Recomendación: no establezca nunca en false.

De forma predeterminada, EnableViewStateMac está establecido en true. Incluso si la aplicación no usa el estado de vista, no establezca EnableViewStateMac en false. Si se establece este valor en false, la aplicación será vulnerable a la creación de scripts entre sitios.

A partir de ASP.NET 4.5.2, el tiempo de ejecución exige **EnableViewStateMac = true**. Incluso si se establece en false, el tiempo de ejecución omite este valor y continúa con el valor establecido en true. Para obtener más información, consulte [ASP.net 4.5.2 y EnableViewStateMac](https://blogs.msdn.com/b/webdev/archive/2014/05/07/asp-net-4-5-2-and-enableviewstatemac.aspx).

En el ejemplo siguiente se muestra cómo establecer EnableViewStateMac en true. En realidad, no es necesario establecer este valor en true porque es true de forma predeterminada. Sin embargo, si se ha establecido en false en cualquier página de la aplicación, debe corregir este valor inmediatamente.

[!code-aspx[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample9.aspx)]

<a id="medium"></a>

### <a name="medium-trust"></a>Confianza media

Recomendación: no dependa de la confianza media (o cualquier otro nivel de confianza) como límite de seguridad.

La confianza parcial no protege la aplicación de forma adecuada y no debe usarse. En su lugar, utilice plena confianza y aísle las aplicaciones que no son de confianza en grupos de aplicaciones independientes. Además, ejecute cada grupo de aplicaciones con una identidad única. Para obtener más información, consulte [confianza parcial de ASP.net no garantiza el aislamiento de aplicaciones](https://support.microsoft.com/kb/2698981).

<a id="appsettings"></a>

### <a name="ltappsettingsgt"></a>&lt;appSettings&gt;

Recomendación: no deshabilite la configuración de seguridad en &lt;elemento&gt; appSettings.

El elemento appSettings contiene muchos valores que son necesarios para las actualizaciones de seguridad. No debe cambiar o deshabilitar estos valores. Si debe deshabilitar estos valores al implementar una actualización, vuelva a habilitar inmediatamente después de completar la implementación.

Para obtener más información, consulte [elemento ASP.net appSettings](https://msdn.microsoft.com/library/hh975440.aspx).

<a id="urlpathencode"></a>

### <a name="urlpathencode"></a>UrlPathEncode

Recomendación: use [UrlEncode](https://msdn.microsoft.com/library/zttxte6w.aspx) en su lugar.

El método UrlPathEncode se ha agregado al .NET Framework para resolver un problema de compatibilidad de explorador muy específico. No codifica adecuadamente una dirección URL y no protege la aplicación de scripting entre sitios. Nunca debe usarlo en la aplicación. En su lugar, use [UrlEncode](https://msdn.microsoft.com/library/zttxte6w.aspx).

En el ejemplo siguiente se muestra cómo pasar una dirección URL codificada como un parámetro de cadena de consulta para un control de hipervínculo.

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample10.cs)]

<a id="performance"></a>

## <a name="reliability-and-performance"></a>Confiabilidad y rendimiento

<a id="presend"></a>

### <a name="presendrequestheaders-and-presendrequestcontent"></a>PreSendRequestHeaders y PreSendRequestContent

Recomendación: no use estos eventos con módulos administrados. En su lugar, escriba un módulo de IIS nativo para realizar la tarea necesaria. Vea [crear módulos http de código nativo](https://msdn.microsoft.com/library/ms693629.aspx).

Puede usar los eventos [PreSendRequestHeaders](https://msdn.microsoft.com/library/system.web.httpapplication.presendrequestheaders.aspx) y [PreSendRequestContent](https://msdn.microsoft.com/library/system.web.httpapplication.presendrequestcontent.aspx) con los módulos nativos de IIS.
> [!WARNING]
> No utilice `PreSendRequestHeaders` y `PreSendRequestContent` con módulos administrados que implementan `IHttpModule`. Al establecer estas propiedades, pueden producirse problemas con las solicitudes asincrónicas. La combinación de enrutamiento solicitado de aplicación (ARR) y WebSockets podría provocar excepciones de infracción de acceso que pueden hacer que w3wp se bloquee. Por ejemplo, iiscore! W3_CONTEXT_BASE:: GetIsLastNotification + 68 en iiscore. dll ha provocado una excepción de infracción de acceso (0xC0000005).

<a id="asyncevents"></a>

### <a name="asynchronous-page-events-with-web-forms"></a>Eventos de página asincrónica con formularios Web Forms

Recomendación: en formularios Web Forms, evite escribir métodos void void para eventos del ciclo de vida de la página y, en su lugar, use [Page. RegisterAsyncTask](https://msdn.microsoft.com/library/system.web.ui.page.registerasynctask.aspx) para el código asincrónico.

Cuando se marca un evento de página con **Async** y **void**, no se puede determinar cuándo ha finalizado el código asincrónico. En su lugar, use Page. RegisterAsyncTask para ejecutar el código asincrónico de una forma que le permita realizar un seguimiento de su finalización.

En el ejemplo siguiente se muestra un controlador de clic de botón que contiene código asincrónico. En este ejemplo se incluye la lectura asincrónica de un valor de cadena, que se proporciona solo como un ejemplo simplificado de una tarea asincrónica y no como práctica recomendada.

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample11.cs)]

Si usa tareas asincrónicas, establezca la plataforma de destino de tiempo de ejecución http en 4,5 (o posterior) en el archivo Web. config. Al establecer la versión de .NET Framework de destino en 4,5, se activa el nuevo contexto de sincronización que se agregó en .NET 4,5. Este valor se establece de forma predeterminada en los nuevos proyectos de Visual Studio, pero no se establece si se trabaja con un proyecto existente.

[!code-xml[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample12.xml)]

<a id="fire"></a>

### <a name="fire-and-forget-work"></a>Trabajo de incendio y olvidar

Recomendación: al controlar una solicitud en ASP.NET, evite iniciar el trabajo de desencadenamiento y olvidar (llamando al método ThreadPool. QueueUserWorkItem o creando un temporizador que llame repetidamente a un delegado).

Si la aplicación tiene un trabajo de desencadenamiento y olvidar que se ejecuta dentro de ASP.NET, la aplicación puede dejar de estar sincronizada. En cualquier momento, el dominio de la aplicación se puede destruir, lo que significa que el proceso en curso puede dejar de coincidir con el estado actual de la aplicación.

Debe trasladar este tipo de trabajo fuera de ASP.NET. Puede usar trabajos Web, un servicio de Windows o un rol de trabajo en Azure para realizar el trabajo en curso y ejecutar ese código desde otro proceso.

Si debe realizar este trabajo en ASP.NET, puede Agregar el paquete Nuget llamado [Webbackgrounder](http://www.nuget.org/packages/webbackgrounder) para ejecutar el código.

<a id="requestentity"></a>

### <a name="request-entity-body"></a>Cuerpo de la entidad de solicitud

Recomendación: Evite leer request. Form o request. InputStream antes del evento Execute del controlador.

Lo más antiguo que debe leer de request. Form o request. InputStream es durante el evento Execute del controlador. En MVC, el controlador es el controlador y el evento Execute es cuando se ejecuta el método de acción. En los formularios Web Forms, la página es el controlador y el evento Execute es cuando se desencadena el evento Page. init. Si lee el cuerpo de la entidad de solicitud antes que el evento Execute, interferirá con el procesamiento de la solicitud.

Si necesita leer el cuerpo de la entidad de solicitud antes del evento Execute, use [request. GetBufferlessInputStream](https://msdn.microsoft.com/library/ff406798.aspx) o [request. GetBufferedInputStream](https://msdn.microsoft.com/library/system.web.httprequest.getbufferedinputstream.aspx). Cuando se usa GetBufferlessInputStream, se obtiene el flujo sin formato de la solicitud y se asume la responsabilidad de procesar la solicitud completa. Después de llamar a GetBufferlessInputStream, Request. Form y request. InputStream no están disponibles porque no se han rellenado con ASP.NET. Cuando se usa GetBufferedInputStream, se obtiene una copia de la secuencia de la solicitud. Request. Form y request. InputStream siguen estando disponibles posteriormente en la solicitud porque ASP.NET rellena la otra copia.

<a id="redirect"></a>

### <a name="responseredirect-and-responseend"></a>Response. Redirect y Response. end

Recomendación: tenga en cuenta las diferencias en la forma en que se controla el subproceso después de llamar a [Response. Redirect (cadena)](https://msdn.microsoft.com/library/t9dwyts4.aspx).

El método [Response. Redirect (String)](https://msdn.microsoft.com/library/t9dwyts4.aspx) llama al método Response. end. En un proceso sincrónico, al llamar a request. Redirect, el subproceso actual se anula inmediatamente. Sin embargo, en un proceso asincrónico, al llamar a Response. Redirect no se anula el subproceso actual, por lo que la ejecución del código continúa para la solicitud. En un proceso asincrónico, debe devolver la tarea desde el método para detener la ejecución del código.

En un proyecto de MVC, no debe llamar a Response. Redirect. En su lugar, devuelva un RedirectResult.

<a id="viewstatemode"></a>

### <a name="enableviewstate-and-viewstatemode"></a>EnableViewState y ViewStateMode

Recomendación: Use ViewStateMode, en lugar de EnableViewState, para proporcionar un control granular sobre qué controles usan el estado de vista.

Cuando se establece EnableViewState en false en la Directiva de página, el estado de vista está deshabilitado para todos los controles de la página y no se puede habilitar. Si desea habilitar el estado de vista solo para determinados controles de la página, establezca ViewStateMode en deshabilitado para la página.

[!code-aspx[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample13.aspx)]

A continuación, establezca ViewStateMode en habilitado solo en los controles que realmente necesiten el estado de vista.

[!code-aspx[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample14.aspx)]

Al habilitar el estado de vista solo para los controles que lo necesiten, puede reducir el tamaño del estado de vista de las páginas Web.

<a id="sqlprovider"></a>

### <a name="sqlmembershipprovider"></a>SqlMembershipProvider

Recomendación: Use Proveedores universales.

En las plantillas de proyecto actuales, SqlMembershipProvider se ha reemplazado por [proveedores universales de ASP.net](http://www.nuget.org/packages/Microsoft.AspNet.Providers), que está disponible como un paquete NuGet. Si usa SqlMembershipProvider en un proyecto que se compiló con una versión anterior de las plantillas, debe cambiar a Proveedores universales. El Proveedores universales trabajar con todas las bases de datos que son compatibles con Entity Framework.

Para obtener más información, consulte [introducing proveedores universales de ASP.net](http://www.hanselman.com/blog/IntroducingSystemWebProvidersASPNETUniversalProvidersForSessionMembershipRolesAndUserProfileOnSQLCompactAndSQLAzure.aspx).

<a id="long"></a>

### <a name="long-running-requests-110-seconds"></a>Solicitudes de ejecución prolongada (> 110 segundos)

Recomendación: use [WebSockets](https://msdn.microsoft.com/library/system.net.websockets.websocket.aspx) o [signalr](../../../signalr/index.md) para clientes conectados y use operaciones de e/s asincrónicas.

Las solicitudes de ejecución prolongada pueden producir resultados imprevisibles y un rendimiento deficiente en la aplicación Web. La configuración de tiempo de espera predeterminada para una solicitud es de 110 segundos. Si usa el estado de sesión con una solicitud de ejecución prolongada, ASP.NET liberará el bloqueo en el objeto de sesión después de 110 segundos. Sin embargo, la aplicación puede estar en medio de una operación en el objeto de sesión cuando se libera el bloqueo y la operación podría no completarse correctamente. Si se bloquea una segunda solicitud del usuario mientras se ejecuta la primera solicitud, la segunda solicitud podría tener acceso al objeto de sesión en un estado incoherente.

Si la aplicación incluye operaciones de e/s de bloqueo (o sincrónicas), la aplicación no responderá.

Para mejorar el rendimiento, use las operaciones de e/s asincrónicas en el .NET Framework. Además, utilice WebSockets o Signalr para conectar clientes al servidor. Estas características están diseñadas para controlar de forma eficaz las solicitudes de ejecución prolongada.
