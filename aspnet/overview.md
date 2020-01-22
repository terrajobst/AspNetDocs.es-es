---
uid: overview
title: Información general de ASP.NET | Microsoft Docs
author: rick-anderson
description: Introducción a ASP.NET, un marco gratuito para la creación de sitios web, aplicaciones web y API Web.
ms.assetid: 3a309468-f1ca-4e51-b9c3-536af79d7a8b
ms.author: riande
ms.date: 08/10/2019
msc.legacyurl: ''
msc.type: content
ms.openlocfilehash: aa4f627bca99f0a7ffbbb53ea45ebdcf0850fd89
ms.sourcegitcommit: 88fc80e3f65aebdf61ec9414810ddbc31c543f04
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/22/2020
ms.locfileid: "76519367"
---
# <a name="aspnet-overview"></a>Información general de ASP.NET

ASP.NET es un marco web gratuito para crear excelentes sitios web y aplicaciones web mediante HTML, CSS y JavaScript. También puede crear API Web y usar tecnologías en tiempo real como Sockets Web.

[ASP.net Core](https://docs.microsoft.com/aspnet/core/) es una alternativa a ASP.net.  Vea las [instrucciones sobre cómo elegir entre ASP.net y ASP.net Core](https://docs.microsoft.com/aspnet/core/choose-aspnet-framework).

## <a name="get-started"></a>Primeros pasos

Instale [Visual Studio 2019](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2019) Community Edition, un IDE gratuito para ASP.net en Windows.

## <a name="websites-and-web-applications"></a>Websites y aplicaciones Web

 ASP.NET ofrece tres marcos para crear aplicaciones web: formularios Web Forms, ASP.NET MVC y ASP.NET Web Pages. Los tres marcos son estables y están maduros, y puede crear excelentes aplicaciones web con cualquiera de ellos. Independientemente del marco que elija, obtendrá todas las ventajas y características de ASP.NET Everywhere.

Cada marco de trabajo tiene como destino un estilo de desarrollo diferente. El que elija depende de una combinación de los recursos de programación (conocimiento, conocimientos y experiencia de desarrollo), el tipo de aplicación que está creando y el enfoque de desarrollo con el que está familiarizado.

A continuación se muestra información general de cada una de las plataformas y algunas ideas sobre cómo elegir entre ellas. Si prefiere un vídeo de introducción, consulte [creación de sitios web con ASP.net](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/Making-Websites-with-ASPNET) y [¿Qué son las herramientas web?](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/what-is-web-tools)

|   | Si tiene experiencia en | Estilo de desarrollo | Conocimientos |
|-----------|----------------------|-----------------------------------------------------|----------------|
| formularios Web Forms | Windows Forms, WPF, .NET | Desarrollo rápido con una rica biblioteca de controles que encapsulan el marcado HTML | RAD avanzado de nivel medio |
| MVC       | Ruby on Rails, .NET  | Control total sobre el marcado HTML, el código y el marcado separados y las pruebas fáciles de escribir. La mejor opción para aplicaciones móviles y de una sola página (SPA). | Nivel intermedio, avanzado |
| Páginas web  | ASP clásico, PHP     | Marcado HTML y el código juntos en el mismo archivo | Nuevo nivel intermedio |

### <a name="web-forms"></a>formularios Web Forms

Con los formularios Web Forms ASP.NET, puede crear sitios web dinámicos con un modelo familiar basado en eventos de arrastrar y colocar. Una superficie diseño y cientos de controles y componentes le permiten crear rápidamente sitios potentes y sofisticados sitios controlados por IU con datos.

[Más información sobre formularios Web Forms](web-forms/index.md)

### <a name="mvc"></a>MVC

ASP.NET MVC ofrece una eficaz forma de compilar sitios web dinámicos basada en modelos, lo que permite una separación clara de intereses y aporta control total sobre el marcado para lograr un desarrollo ameno y rápido. ASP.NET MVC incluye muchas características que permiten el desarrollo para TDD rápido para crear aplicaciones sofisticadas que usan los estándares web más recientes.

[Más información sobre MVC](mvc/index.md)

### <a name="aspnet-web-pages"></a>ASP.NET Web Pages

ASP.NET Web Pages y el sintaxis Razor proporcionan una manera rápida, accesible y ligera de combinar código de servidor con HTML para crear contenido Web dinámico. Conéctese a bases de datos, agregue vídeo, vínculos a sitios de redes sociales e incluya muchas más características que le ayuden a crear sitios atractivos que cumplan los estándares web más recientes.

[Más información acerca de las páginas web](web-pages/index.md)

### <a name="notes-about-web-forms-mvc-and-web-pages"></a>Notas sobre formularios Web Forms, MVC y Web pages

Los tres marcos de ASP.NET se basan en el .NET Framework y comparten la funcionalidad básica de .NET y de ASP.NET. Por ejemplo, los tres marcos ofrecen un modelo de seguridad de inicio de sesión basado en la pertenencia, y los tres comparten las mismas funciones para administrar solicitudes, controlar sesiones, etc., que forman parte de la funcionalidad básica de ASP.NET.

Además, los tres marcos no son totalmente independientes y elegir uno no impide el uso de otro. Dado que los marcos de trabajo pueden coexistir en la misma aplicación Web, no es raro ver componentes individuales de aplicaciones escritas con diferentes marcos. Por ejemplo, las partes orientadas al cliente de una aplicación pueden desarrollarse en MVC para optimizar el marcado, mientras que el acceso a datos y las partes administrativas se desarrollan en formularios Web para aprovechar los controles de datos y el acceso a datos simple.

## <a name="web-apis"></a>API web

ASP.NET Web API es un marco que facilita la creación de servicios HTTP disponibles para una amplia variedad de clientes, entre los que se incluyen exploradores y dispositivos móviles. ASP.NET Web API es la plataforma perfecta para crear aplicaciones RESTful en .NET Framework.

[Más información acerca de Web API](web-api/index.md)

<!-- Put first under Web API TOC:  Watch video (9 minutes) https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/services-and-aspnet -->

## <a name="real-time-technologies"></a>Tecnologías en tiempo real

ASP.NET Signalr es una nueva biblioteca para desarrolladores de ASP.NET que facilita el desarrollo de funcionalidades Web en tiempo real. Signalr permite la comunicación bidireccional entre el servidor y el cliente. Los servidores pueden insertar contenido en los clientes conectados al instante a medida que esté disponible. Signalr admite Sockets web y recurre a otras técnicas compatibles para los exploradores más antiguos. Signalr incluye API para la administración de conexiones (por ejemplo, eventos de conexión y desconexión), agrupación de conexiones y autorización.

[Más información acerca de Signalr](signalr/index.md)

<!-- Put first under SignalR TOC:  Watch video (6 minutes) https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/signalr-and-the-real-time-web -->

## <a name="mobile-apps-and-sites"></a>Aplicaciones y sitios móviles

ASP.NET puede potenciar las aplicaciones móviles nativas con un back-end de API Web, así como los sitios web móviles con marcos de diseño con capacidad de respuesta como Twitter bootstrap. Si va a compilar una aplicación móvil nativa, es fácil crear una API Web basada en JSON para controlar el acceso a los datos, la autenticación y las notificaciones de envío de la aplicación. Si va a crear un sitio móvil con capacidad de respuesta, puede usar cualquier marco CSS o sistema de cuadrícula abierto que prefiera, o bien seleccionar un sistema móvil eficaz como jQuery Mobile o Sencha y excelentes aplicaciones móviles con PhoneGap.

[Más información sobre el desarrollo de aplicaciones móviles y sitios](mobile/overview.md)

<!-- Put first under mobile TOC:  Watch video (11 minutes) https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/aspnet-and-mobile -->

## <a name="single-page-applications"></a>Aplicación de página única

ASP.NET aplicación de una sola página (SPA) le ayuda a crear aplicaciones que incluyen interacciones del lado cliente significativas con HTML 5, CSS 3 y JavaScript. Visual Studio incluye una plantilla para compilar aplicaciones de una sola página mediante Knockout. js y ASP.NET Web API. Además de la plantilla integrada SPA, también se pueden descargar plantillas de SPA creadas por la comunidad.

[Más información sobre el desarrollo de aplicaciones de una sola página](single-page-application/index.md)

## <a name="webhooks"></a>WebHooks

Webhooks es un patrón de HTTP ligero que proporciona un modelo pub/sub sencillo para el cableado de las API Web y los servicios SaaS. Cuando se produce un evento en un servicio, se envía una notificación en forma de una solicitud HTTP POST a los suscriptores registrados. La solicitud POST contiene información sobre el evento, lo que hace posible que el receptor actúe en consecuencia.

Los webhooks se exponen mediante un gran número de servicios, como Dropbox, GitHub, Instagram, MailChimp, PayPal, la demora, Trello y muchos otros. Por ejemplo, un webhook puede indicar que un archivo ha cambiado en Dropbox o que se ha confirmado un cambio de código en GitHub o que se ha iniciado un pago en PayPal o que se ha creado una tarjeta en Trello.

[Más información sobre webhooks](webhooks/index.md)

<!--
Create Deployment TOC based on https://www.asp.net/aspnet/overview/deployment
Copy deployment content map to MVC, WebForms, Web Pages, Web API sections.
Copy Web Deployment in Enterprise from WebForms to MVC
Move under ASP.NET Best practices
    What not to do in ASP.NET, and what to do instead https://review.docs.microsoft.cus/aspnet/aspnet/overview/web-development-best-practices/what-not-to-do-in-aspnet-and-what-to-do-instead
    Async and await https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/async-and-await
    Building Real World Cloud Apps with Azure https://review.docs.microsoft.com/aspnet/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/introduction
    Hands on Lab: Maintainable Azure Websites: Managing Change and Scale https://review.docs.microsoft.com/aspnet/aspnet/overview/developing-apps-with-windows-azure/maintainable-azure-websites-managing-change-and-scale

-->
