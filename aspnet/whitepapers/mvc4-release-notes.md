---
uid: whitepapers/mvc4-release-notes
title: ASP.NET MVC 4 | Microsoft Docs
author: rick-anderson
description: En este documento se describe la versión de ASP.NET MVC 4.
ms.author: riande
ms.date: 09/09/2011
ms.assetid: f014524f-25c0-4094-b8e1-886d99536f00
msc.legacyurl: /whitepapers/mvc4-release-notes
msc.type: content
ms.openlocfilehash: b57480bd0274fbb76c600dfb0dd09037bdcbf1e7
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78454237"
---
# <a name="aspnet-mvc-4"></a>ASP.NET MVC 4

> En este documento se describe la versión de ASP.NET MVC 4.

- [Notas de instalación](#_Toc303253802)
- [Documentación](#_Toc303253803)
- [Compatibilidad](#_Toc303253804)
- [Requisitos de software](#_Toc303253805)
- [Nuevas características de ASP.NET MVC 4](#_Toc303253807)

    - [ASP.NET Web API](#_Toc317096197) (Más información sobre ASP.NET Web API)
    - [Mejoras en las plantillas de proyecto predeterminadas](#_Toc303253808)
    - [Plantilla de proyecto móvil](#_Toc303253809)
    - [Modos de presentación](#_Toc303253810)
    - [jQuery Mobile, el modificador de vista y el reemplazo del explorador](#_Toc303253811)
    - [Compatibilidad de tareas con controladores asincrónicos](#_Toc303253813)
    - [SDK de Azure](#_Toc303253814)
    - [Migraciones de bases de datos](#_Toc303253818)
    - [Plantilla de proyecto vacía](#_Toc303253819)
    - [Agregar controlador a cualquier carpeta de proyecto](#_Toc303253820)
    - [Unión y minificación](#_Toc303253821)
    - [Habilitación de inicios de sesión desde Facebook y otros sitios mediante OAuth y OpenID](#_Toc303253822)
- [Actualización de un proyecto de ASP.NET MVC 3 a ASP.NET MVC 4](#_Toc303253806)
- [Cambios de ASP.NET MVC 4 Release Candidate](#_Toc303253817)
- [Problemas conocidos y cambios importantes](#_Toc303253815)

<a id="_Toc303253802"></a>
## <a name="installation-notes"></a>Notas de la instalación

ASP.NET MVC 4 para Visual Studio 2010 se puede instalar desde la [Página principal de ASP.NET MVC 4](../mvc/mvc4.md) mediante el instalador de plataforma Web.

Se recomienda desinstalar las vistas previas instaladas anteriormente de ASP.NET MVC 4 antes de instalar ASP.NET MVC 4. Puede actualizar ASP.NET MVC 4 beta y Release Candidate a ASP.NET MVC 4 sin necesidad de desinstalar.

Esta versión no es compatible con las versiones preliminares de .NET Framework 4,5. Debe actualizar por separado las versiones de vista previa instaladas de .NET Framework 4,5 a la versión final antes de instalar ASP.NET MVC 4.

ASP.NET MVC 4 se puede instalar y ejecutar en paralelo con ASP.NET MVC 3.

<a id="_Toc303253803"></a>
## <a name="documentation"></a>Documentación

La documentación de ASP.NET MVC está disponible en el sitio web de MSDN en la dirección URL siguiente:

[https://go.microsoft.com/fwlink/?LinkID=243043](https://go.microsoft.com/fwlink/?LinkID=243043)

Los tutoriales y otra información sobre ASP.NET MVC están disponibles en la página de MVC 4 del sitio web de ASP.NET ([https://www.asp.net/mvc/mvc4](../mvc/mvc4.md)).

<a id="_Toc303253804"></a>
## <a name="support"></a>Compatibilidad

ASP.NET MVC 4 es totalmente compatible. Si tiene preguntas sobre cómo trabajar con esta versión, también puede publicarlas en el foro de ASP.NET MVC ([https://forums.asp.net/1146.aspx](https://forums.asp.net/1146.aspx)), donde los miembros de la comunidad de ASP.net suelen ser capaces de proporcionar soporte técnico informal.

<a id="_Toc303253805"></a>
## <a name="software-requirements"></a>Requisitos de software

Los componentes de ASP.NET MVC 4 para Visual Studio requieren PowerShell 2,0 y Visual Studio 2010 con Service Pack 1 o Visual Web Developer Express 2010 con Service Pack 1.

<a id="_Toc303253807"></a>
## <a name="new-features-in-aspnet-mvc-4"></a>Nuevas características de ASP.NET MVC 4

En esta sección se describen las características que se han introducido en la versión ASP.NET MVC 4.

<a id="_Toc317096197"></a>
### <a name="aspnet-web-api"></a>ASP.NET Web API

ASP.NET MVC 4 incluye ASP.NET Web API, un nuevo marco de trabajo para crear servicios HTTP que pueden llegar a una amplia gama de clientes, incluidos exploradores y dispositivos móviles. ASP.NET Web API también es una plataforma ideal para la creación de servicios RESTful.

ASP.NET Web API incluye compatibilidad con las siguientes características:

- **Modelo de programación http moderno:** Acceder y manipular directamente las solicitudes y respuestas HTTP en las API Web con un nuevo modelo de objetos HTTP fuertemente tipado. El mismo modelo de programación y la canalización HTTP están disponibles de forma simétrica en el cliente a través del nuevo tipo *HttpClient* .
- **Compatibilidad total con las rutas:** ASP.NET Web API admite el conjunto completo de funcionalidades de ruta de enrutamiento de ASP.NET, incluidas las restricciones y los parámetros de ruta. Además, use convenciones simples para asignar acciones a métodos HTTP.
- **Negociación de contenido:** El cliente y el servidor pueden trabajar conjuntamente para determinar el formato correcto de los datos que se devuelven desde una API Web. ASP.NET Web API proporciona compatibilidad predeterminada con los formatos codificados de URL de formulario, JSON y XML, y puede ampliar esta compatibilidad agregando sus propios formateadores, o incluso reemplazando la estrategia predeterminada de negociación de contenido.
- **Enlace y validación del modelo:** Los enlazadores de modelos proporcionan una manera sencilla de extraer datos de varias partes de una solicitud HTTP y convertir esas partes de mensaje en objetos .NET que pueden usar las acciones de la API Web. La validación también se realiza en parámetros de acción basados en anotaciones de datos.
- **Filtros:** ASP.NET Web API admite filtros, incluidos filtros conocidos como el atributo *[Authorize]* . Puede crear y conectar sus propios filtros para las acciones, la autorización y el control de excepciones.
- **Composición de consultas:** Use el atributo de filtro *[Queryable]* en una acción que devuelva *IQueryable* para habilitar la compatibilidad con la consulta de la API Web a través de las convenciones de consulta de oData.
- Mejora de la capacidad de **prueba:** En lugar de establecer los detalles de HTTP en objetos de contexto estáticos, las acciones de la API Web funcionan con instancias de *HttpRequestMessage* y *HttpResponseMessage*. Cree un proyecto de prueba unitaria junto con el proyecto de API Web para empezar a escribir rápidamente pruebas unitarias para la funcionalidad de la API Web.
- **Configuración basada en código:** ASP.NET Web API configuración se realiza únicamente a través del código, lo que hace que los archivos de configuración estén limpios. Use el patrón de localizador de servicio proporcionado para configurar los puntos de extensibilidad.
- **Compatibilidad mejorada con los contenedores de inversión de control (IOC):** ASP.NET Web API proporciona una gran compatibilidad con los contenedores de IoC a través de una abstracción mejorada de la resolución de dependencias
- **Self-host:** Las API Web se pueden hospedar en su propio proceso, además de en IIS, al mismo tiempo que se sigue usando todo el potencial de rutas y otras características de Web API.
- **Crear páginas de ayuda y de prueba personalizadas:** Ahora puede crear fácilmente páginas de ayuda y prueba personalizadas para las API Web mediante el nuevo servicio *IApiExplorer* para obtener una descripción completa del tiempo de ejecución de las API Web.
- **Supervisión y diagnóstico:** ASP.NET Web API ahora proporciona una infraestructura ligera de seguimiento que facilita la integración con soluciones de registro existentes como System. Diagnostics, ETW y marcos de registro de terceros. Puede habilitar el seguimiento si proporciona una implementación de *ITraceWriter* y la agrega a la configuración de la API Web.
- **Generación de vínculos:** Use el ASP.NET Web API *UrlHelper* para generar vínculos a los recursos relacionados en la misma aplicación.
- **Plantilla de proyecto de API Web:** Seleccione el nuevo proyecto de Web API y el nuevo Asistente para proyectos de MVC 4 para empezar a trabajar rápidamente con ASP.NET Web API.
- **Scaffolding:** Use el cuadro de diálogo **Agregar controlador** para aplicar scaffolding a un controlador de API Web basado en un tipo de modelo basado en Entity Framework.

Para obtener más detalles sobre ASP.NET Web API visite [https://www.asp.net/web-api](../web-api/index.md).

<a id="_Toc303253808"></a>
### <a name="enhancements-to-default-project-templates"></a>Mejoras en las plantillas de proyecto predeterminadas

La plantilla que se usa para crear nuevos proyectos de ASP.NET MVC 4 se ha actualizado para crear un sitio web de aspecto más moderno:

![](mvc4-release-notes/_static/image1.png)

Además de las mejoras cosméticas, hay una funcionalidad mejorada en la nueva plantilla. La plantilla emplea una técnica denominada representación adaptable para tener un buen aspecto en exploradores de escritorio y exploradores móviles sin ninguna personalización.

![](mvc4-release-notes/_static/image2.png)

Para ver la representación adaptable en acción, puede usar un emulador móvil o simplemente intentar cambiar el tamaño de la ventana del explorador de escritorio para que sea menor. Cuando la ventana del explorador se quede lo suficientemente pequeña, cambiará el diseño de la página.

<a id="_Toc303253809"></a>
### <a name="mobile-project-template"></a>Plantilla de proyecto móvil

Si va a iniciar un nuevo proyecto y desea crear un sitio específicamente para los exploradores móviles y de Tablet PC, puede utilizar la nueva plantilla de proyecto de aplicación móvil. Esto se basa en jQuery Mobile, una biblioteca de código abierto para compilar la interfaz de usuario optimizada para toque:

![](mvc4-release-notes/_static/image3.png)

Esta plantilla contiene la misma estructura de aplicación que la plantilla de aplicación de Internet (y el código del controlador es prácticamente idéntico), pero tiene estilo con jQuery Mobile para tener buenos problemas y comportarse bien en dispositivos móviles basados en el toque. Para obtener más información sobre la estructura y el estilo de la interfaz de usuario móvil, vea el [sitio web de jQuery Mobile Project](http://jquerymobile.com/).

Si ya tiene un sitio orientado al escritorio al que desea agregar vistas optimizadas para dispositivos móviles, o si desea crear un único sitio que sirva vistas de estilo diferente para los exploradores de escritorio y móviles, puede usar la nueva característica modos de presentación. (Consulte la sección siguiente).

<a id="_Toc303253810"></a>
### <a name="display-modes"></a>Modos de presentación

La nueva característica modos de presentación permite a una aplicación seleccionar vistas según el explorador que realiza la solicitud. Por ejemplo, si un explorador de escritorio solicita la Página principal, la aplicación podría usar la plantilla Views\Home\Index.cshtml. Si un explorador móvil solicita la Página principal, es posible que la aplicación devuelva la plantilla Views\Home\Index.mobile.cshtml.

Los diseños y las particiones también se pueden invalidar para determinados tipos de explorador. Por ejemplo:

- Si la carpeta Views\Shared contiene las plantillas \_layout. cshtml y \_layout. Mobile. cshtml, de forma predeterminada, la aplicación usará \_layout. Mobile. cshtml durante las solicitudes de los exploradores móviles y \_layout. cshtml durante otras solicitudes.
- Si una carpeta contiene \_Part. cshtml y \_perpartal. Mobile. cshtml, el @Html.Partialde instrucciones ("\_perparted") representará \_subpartal. Mobile. cshtml durante las solicitudes de los exploradores móviles y \_parcialmente. cshtml durante otras solicitudes.

Si desea crear vistas, diseños o vistas parciales más específicos para otros dispositivos, puede registrar una nueva instancia de *DefaultDisplayMode* para especificar el nombre que se va a buscar cuando una solicitud cumple determinadas condiciones. Por ejemplo, puede Agregar el código siguiente al método de *Inicio de la aplicación\_* en el archivo global. asax para registrar la cadena "iPhone" como modo de presentación que se aplica cuando el explorador de iPhone de Apple realiza una solicitud:

[!code-csharp[Main](mvc4-release-notes/samples/sample1.cs)]

Después de que se ejecute este código, cuando un explorador de iPhone de Apple realice una solicitud, la aplicación usará el diseño Views\Shared\\_Layout. iPhone. cshtml (si existe). Para obtener más información sobre el modo de presentación, vea [ASP.NET MVC 4 Mobile Features](../mvc/overview/older-versions/aspnet-mvc-4-mobile-features.md). Las aplicaciones que usan DisplayModeProvider deben instalar el paquete de NuGet [DisplayModes fijo](http://nuget.org/packages/Microsoft.AspNet.Mvc.FixedDisplayModes) . La [actualización de ASP.NET 2012](https://go.microsoft.com/fwlink/?LinkID=271322) incluye el paquete de NuGet [DisplayModes fijo](http://nuget.org/packages/Microsoft.AspNet.Mvc.FixedDisplayModes) en las nuevas plantillas de proyecto. Consulte [ASP.NET MVC 4 Mobile Caching error corregido](https://blogs.msdn.com/b/rickandy/archive/2012/09/17/asp-net-mvc-4-mobile-caching-bug-fixed.aspx) para obtener más información sobre la corrección.

<a id="_Toc303253811"></a>
### <a name="jquery-mobile-and-mobile-features"></a>Características móviles y móviles de jQuery

Para obtener información sobre cómo compilar aplicaciones móviles con ASP.NET MVC 4 mediante jQuery Mobile, vea el tutorial [ASP.NET MVC 4 Mobile Features](../mvc/overview/older-versions/aspnet-mvc-4-mobile-features.md).

<a id="_Toc303253813"></a>
### <a name="task-support-for-asynchronous-controllers"></a>Compatibilidad de tareas con controladores asincrónicos

Ahora puede escribir métodos de acción asincrónicos como métodos únicos que devuelven un objeto de tipo *Task* o *task&lt;ActionResult&gt;* .

 Para obtener más información, vea [usar métodos asincrónicos en ASP.NET MVC 4](../mvc/overview/performance/using-asynchronous-methods-in-aspnet-mvc-4.md).

<a id="_Toc303253814"></a>
### <a name="azure-sdk"></a>SDK de Azure

ASP.NET MVC 4 es compatible con las versiones 1,6 y más recientes del SDK de Windows Azure.

<a id="_Toc303253818"></a>
### <a name="database-migrations"></a>Migraciones de bases de datos

Los proyectos de ASP.NET MVC 4 ahora incluyen Entity Framework 5. Una de las excelentes características de Entity Framework 5 es la compatibilidad con las migraciones de bases de datos. Esta característica permite desarrollar fácilmente el esquema de la base de datos mediante una migración centrada en el código mientras se conservan los datos en la base de datos. Para obtener más información sobre las migraciones de bases de datos, vea [Agregar un nuevo campo a la tabla y al modelo de películas](../mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-new-field-to-the-movie-model-and-table.md) en el [tutorial Introducción a ASP.NET MVC 4](../mvc/overview/older-versions/getting-started-with-aspnet-mvc4/intro-to-aspnet-mvc-4.md).

<a id="_Toc303253819"></a>
### <a name="empty-project-template"></a>Plantilla de proyecto vacía

La plantilla de proyecto vacía de MVC está realmente vacía, por lo que puede empezar desde una pizarra completamente limpia. Se ha cambiado el nombre de la versión anterior de la plantilla de proyecto vacía a básica.

<a id="_Toc303253820"></a>
### <a name="add-controller-to-any-project-folder"></a>Agregar controlador a cualquier carpeta de proyecto

Ahora puede hacer clic con el botón derecho y seleccionar **Agregar controlador** desde cualquier carpeta del proyecto de MVC. Esto le ofrece más flexibilidad para organizar los controladores que desee, incluido el mantenimiento de los controladores de MVC y API Web en carpetas independientes.

<a id="_Toc303253821"></a>
### <a name="bundling-and-minification"></a>Unión y minificación

El marco de trabajo de agrupación y minificación permite reducir el número de solicitudes HTTP que una página web debe realizar mediante la combinación de archivos individuales en un único archivo agrupado para scripts y CSS. A continuación, puede reducir el tamaño total de esas solicitudes minificar el contenido de la agrupación. Minificar puede incluir actividades como la eliminación de espacios en blanco para acortar los nombres de variables e incluso contraer los selectores de CSS en función de su semántica. Las agrupaciones se declaran y configuran en el código y se puede hacer referencia a ellas fácilmente en las vistas a través de métodos auxiliares que pueden generar un solo vínculo a la agrupación o, al depurar, varios vínculos al contenido individual de la agrupación. Para obtener más información [, consulte agrupación y minificación](../mvc/overview/performance/bundling-and-minification.md).

<a id="_Toc303253822"></a>
### <a name="enabling-logins-from-facebook-and-other-sites-using-oauth-and-openid"></a>Habilitación de inicios de sesión desde Facebook y otros sitios mediante OAuth y OpenID

Las plantillas predeterminadas en la plantilla de proyecto de Internet de ASP.NET MVC 4 ahora incluyen compatibilidad con el inicio de sesión de OAuth y OpenID mediante la biblioteca DotNetOpenAuth. Para obtener información sobre la configuración de un proveedor de OAuth o OpenID, consulte [compatibilidad de OAuth/OpenID con WebForms, MVC y páginas web](https://blogs.msdn.com/b/webdev/archive/2012/08/15/oauth-openid-support-for-webforms-mvc-and-webpages.aspx) y la [documentación de las características de OAuth y OpenID en ASP.NET Web pages](../web-pages/overview/releases/top-features-in-web-pages-2.md#oauthsetup).

<a id="_Toc303253806"></a>
## <a name="upgrading-an-aspnet-mvc-3-project-to-aspnet-mvc-4"></a>Actualización de un proyecto de ASP.NET MVC 3 a ASP.NET MVC 4

ASP.NET MVC 4 se puede instalar en paralelo con ASP.NET MVC 3 en el mismo equipo, lo que proporciona flexibilidad a la hora de elegir cuándo actualizar una aplicación ASP.NET MVC 3 a ASP.NET MVC 4.

La manera más sencilla de actualizar es crear un nuevo proyecto de ASP.NET MVC 4 y copiar todas las vistas, controladores, código y archivos de contenido del proyecto de MVC 3 existente en el nuevo proyecto y, a continuación, actualizar las referencias de ensamblado en el nuevo proyecto para que coincidan con cualquier plantilla que no sea de MVC en cluded assembiles está usando. Si ha realizado cambios en el archivo Web. config en el proyecto de MVC 3, también debe combinar esos cambios en el archivo Web. config en el proyecto de MVC 4.

Para actualizar manualmente una aplicación existente de ASP.NET MVC 3 a la versión 4, haga lo siguiente:

1. En todos los archivos Web. config del proyecto (hay uno en la raíz del proyecto, uno en la carpeta vistas y otro en la carpeta vistas para cada área del proyecto), reemplace cada instancia del texto siguiente (Nota: System. Web. Webpages, version = 1.0.0.0 no se encuentra en proyectos creados con Visual Studio 2012): 

    [!code-console[Main](mvc4-release-notes/samples/sample2.cmd)]

    con el siguiente texto correspondiente:

    [!code-console[Main](mvc4-release-notes/samples/sample3.cmd)]
2. En el archivo Web. config raíz, actualice el elemento *Webpages: version* a "2.0.0.0" y agregue una nueva clave *PreserveLoginUrl* que tenga el valor "true": 

    [!code-xml[Main](mvc4-release-notes/samples/sample4.xml)]
3. En Explorador de soluciones, haga clic con el botón derecho en las referencias y seleccione administrar paquetes NuGet. En el panel izquierdo, seleccione **Online\NuGet oficial paquete origen**y, a continuación, actualice lo siguiente:

    - ASP.NET MVC 4
    - (Opcional) jQuery, la validación de jQuery y jQuery UI
    - Opta Entity Framework
    - (Optonal) Modernizr
4. En Explorador de soluciones, haga clic con el botón secundario en el nombre del proyecto y seleccione descargar el proyecto. A continuación, vuelva a hacer clic con el botón derecho en el nombre y seleccione Editar *nombreDeProyecto*. csproj.
5. Busque el elemento *ProjectTypeGuids* y reemplace {E53F8FEA-EAE0-44A6-8774-FFD645390401} por {E3E379DF-F4C6-4180-9B81-6769533ABE47}.
6. Guarde los cambios, cierre el archivo de proyecto (. csproj) que estaba editando, haga clic con el botón derecho en el proyecto y, a continuación, seleccione volver a cargar el proyecto.
7. Si el proyecto hace referencia a cualquier biblioteca de terceros compilada con versiones anteriores de ASP.NET MVC, abra el archivo Web. config raíz y agregue los tres elementos *bindingRedirect* siguientes en la sección de *configuración* : 

    [!code-xml[Main](mvc4-release-notes/samples/sample5.xml)]

<a id="_Toc303253817"></a>
## <a name="changes-from-aspnet-mvc-4-release-candidate"></a>Cambios de ASP.NET MVC 4 Release Candidate

Las notas de la versión de ASP.NET MVC 4 Release Candidate se pueden encontrar aquí:

Los principales cambios de ASP.NET MVC 4 Release Candidate en esta versión se resumen a continuación:

- **Configuración por controlador:** ASP.NET Web API controladores se pueden atribuir con un atributo personalizado que implemente *IControllerConfiguration* para configurar sus propios formateadores, el selector de acciones y los enlazadores de parámetros. Se ha quitado *HttpControllerConfigurationAttribute* .
- **Controladores de mensajes por ruta:** Ahora puede especificar el controlador de mensajes final en la cadena de solicitud para una ruta determinada. Esto permite que los marcos de trabajo de paso a través usen el enrutamiento para enviarse a sus propios puntos de conexión (no*IHttpController*).
- **Notificaciones de progreso:** *ProgressMessageHandler* genera una notificación de progreso para las dos entidades de solicitud que se cargan y las entidades de respuesta que se descargan. Con este controlador es posible realizar un seguimiento de la carga del cuerpo de una solicitud o de la descarga de un cuerpo de respuesta.
- **Contenido de la extracción:** La clase *PushStreamContent* permite escenarios en los que un productor de datos desea escribir directamente en la solicitud o la respuesta (ya sea de forma sincrónica o asincrónica) mediante una secuencia. Cuando *PushStreamContent* está listo para aceptar los datos que llama a un delegado de acción con el flujo de salida. A continuación, el desarrollador puede escribir en la secuencia tanto tiempo como sea necesario y cerrar la secuencia cuando se complete la escritura. *PushStreamContent* detecta el cierre de la secuencia y completa la *tarea* asincrónica subyacente para escribir el contenido.
- **Crear respuestas de error:** Use el tipo *HttpError* para representar de forma coherente la información de error de, como errores de validación y excepciones, al tiempo que sigue teniendo en cuenta el *IncludeErrorDetailPolicy*. Use los nuevos métodos de extensión *CreateErrorResponse* para crear fácilmente respuestas de error con *HttpError* como contenido. El contenido de *HttpError* se negocia por completo.
- **MediaRangeMapping quitado:** Ahora, el negociador de contenido predeterminado controla los intervalos de tipo de medio.
- El **enlace de parámetros predeterminado para los parámetros de tipo simple es ahora [FromUri]:** En versiones anteriores de ASP.NET Web API el enlace de parámetros predeterminado para los parámetros de tipo simple usaban el enlace de modelos. El enlace de parámetros predeterminado para los parámetros de tipo simple es ahora *[FromUri]* .
- La **selección de acciones respeta los parámetros necesarios:** La selección de acciones en ASP.NET Web API ahora solo seleccionará una acción si se proporcionan todos los parámetros necesarios que proceden del URI. Un parámetro se puede especificar como opcional proporcionando un valor predeterminado para el argumento en la Signatura del método de acción.
- **Personalización de enlaces de parámetros http:** Use *ParameterBindingAttribute* para personalizar el enlace de parámetros para un parámetro de acción específico o use *ParameterBindingRules* en el *HttpConfiguration* para personalizar los enlaces de parámetros de manera más amplia.
- **Mejoras en elemento mediatypeformatter:** Los formateadores ahora tienen acceso a la instancia completa de *HttpContent* .
- **Selección de directiva de almacenamiento en búfer de host:** Implemente y configure el servicio *IHostBufferPolicySelector* en ASP.net web API para permitir que los hosts determinen la Directiva en la que se va a usar el almacenamiento en búfer.
- **Acceder a los certificados de cliente de una manera independiente del host:** Use el método de extensión *GetClientCertificate* para obtener el certificado de cliente proporcionado del mensaje de solicitud.
- **Extensibilidad de negociación de contenido:** Personalice la negociación de contenido derivando de *DefaultContentNegotiator* e invalidando cualquier aspecto de la negociación de contenido que le gustaría.
- **Compatibilidad para devolver respuestas 406 no aceptables:** Ahora puede devolver 406 respuestas no aceptables en ASP.NET Web API cuando no se encuentra un formateador adecuado mediante la creación de un *DefaultContentNegotiator* con el parámetro *excludeMatchOnTypeOnly* establecido en *true*.
- **Lea los datos del formulario como NameValueCollection o JToken:** Puede leer los datos del formulario en la cadena de consulta de URI o en el cuerpo de la solicitud como *NameValueCollection* mediante los métodos de extensión *ParseQueryString* y *ReadAsFormDataAsync* , respectivamente. Del mismo modo, puede leer los datos del formulario en la cadena de consulta de URI o en el cuerpo de la solicitud como *JToken* mediante los métodos de extensión *TryReadQueryAsJson* y *ReadAsAsync*&lt;&gt; t, respectivamente.
- **Mejoras de varias partes:** Ahora es posible escribir una *MultipartStreamProvider* que esté completamente adaptada al tipo de datos MIME de varias partes que pueda leer y presente el resultado de la manera óptima para el usuario. También puede enlazar un paso posterior al procesamiento en el *MultipartStreamProvider* que permite que la implementación realice cualquier posterior procesamiento que desee en las partes del cuerpo de varias partes MIME. Por ejemplo, la implementación de *MultipartFormDataStreamProvider* Lee las partes de datos del formulario HTML y las agrega a *NameValueCollection* para que sean fáciles de obtener desde el autor de la llamada.
- **Mejoras en la generación de vínculos:** *UrlHelper* ya no depende de *HttpControllerContext*. Ahora puede tener acceso a *UrlHelper* desde cualquier contexto en el que el *HttpRequestMessage* esté disponible.
- **Cambio en el orden de ejecución del controlador de mensajes:** Los controladores de mensajes se ejecutan ahora en el orden en que se configuran en lugar de hacerlo en orden inverso.
- **Aplicación auxiliar para el cableado de controladores de mensajes:** El nuevo *HttpClientFactory* que puede conectar *DelegatingHandlers* y crear un *HttpClient* con la canalización deseada lista para ir. También proporciona funcionalidad para conectar con controladores internos alternativos (el valor predeterminado es *HttpClientHandler*), así como realizar el cableado al usar *HttpMessageInvoker* u otro *DelegatingHandler* en lugar de *HttpClient* como el invocador superior.
- **Compatibilidad con redes CDN en la optimización web de ASP.net:** La optimización web de ASP.NET ahora proporciona compatibilidad con rutas de acceso alternativas de CDN que permiten especificar para cada paquete una dirección URL adicional que apunta a ese mismo recurso en una red de entrega de contenido. La compatibilidad con redes CDN le permite obtener el script y las agrupaciones de estilo geográficamente más cerca de los consumidores finales de las aplicaciones Web. Las aplicaciones de producción deben implementar una reserva cuando la red CDN no está disponible. Pruebe la reserva.
- **ASP.NET Web API rutas y la configuración se movieron al método estático *WebApiConfig. Register* que se puede volver a usar en el código de prueba.** ASP.NET Web API rutas se agregaron anteriormente en *RouteConfig. RegisterRoutes* junto con las rutas MVC estándar. Las rutas y configuraciones predeterminadas de ASP.NET Web API se administran ahora en un método *WebApiConfig. Register* independiente para facilitar las pruebas.

<a id="_Toc303253815"></a>
## <a name="known-issues-and-breaking-changes"></a>Problemas conocidos y cambios importantes

- **La versión RC y RTM de ASP.NET MVC 4 devolvía incorrectamente las vistas de escritorio en caché cuando se deben devolver vistas móviles.**

    - Consulte [ASP.NET MVC 4 Mobile Caching error corregido](https://blogs.msdn.com/b/rickandy/archive/2012/09/17/asp-net-mvc-4-mobile-caching-bug-fixed.aspx) para obtener más información sobre la corrección. La solución se puede instalar desde el paquete de NuGet [DisplayModes fijo](http://nuget.org/packages/Microsoft.AspNet.Mvc.FixedDisplayModes) .
- **Cambios importantes en el motor de vistas de Razor**. Se han quitado los siguientes tipos de *System. Web. Mvc. Razor*: 

    - *ModelSpan*
    - *MvcVBRazorCodeGenerator*
    - *MvcCSharpRazorCodeGenerator*
    - *MvcVBRazorCodeParser*

  También se han quitado los siguientes métodos: 

    - *MvcCSharpRazorCodeParser. ParseInheritsStatement (System. Web. Razor. Parser. CodeBlockInfo)*
    - *MvcWebPageRazorHost. DecorateCodeGenerator (System. Web. Razor. Generator. RazorCodeGenerator)*
    - *MvcVBRazorCodeParser. ParseInheritsStatement (System. Web. Razor. Parser. CodeBlockInfo)*
- **Cuando WebMatrix. webdata. dll se incluye en el directorio/bin de una aplicación ASP.NET MVC 4, toma la dirección URL de la autenticación de formularios.** Al agregar el ensamblado WebMatrix. webdata. dll a la aplicación (por ejemplo, al seleccionar "ASP.NET Web Pages con sintaxis Razor" al usar el cuadro de diálogo Agregar dependencias que se pueden implementar), se invalidará la redirección de inicio de sesión de autenticación a/Account/Logon en lugar de/Account/login según lo esperado por el controlador de cuentas predeterminado de ASP.NET MVC. Para evitar este comportamiento y usar la dirección URL especificada ya en la sección de autenticación de Web. config, puede Agregar una appSetting denominada PreserveLoginUrl y establecerla en true: 

    [!code-xml[Main](mvc4-release-notes/samples/sample6.xml)]
- **No se puede instalar el administrador de paquetes NuGet al intentar instalar ASP.NET MVC 4 para instalaciones en paralelo de Visual Studio 2010 y Visual Web Developer 2010.** Para ejecutar Visual Studio 2010 y Visual Web Developer 2010 en paralelo con ASP.NET MVC 4, debe instalar ASP.NET MVC 4 después de que ya se hayan instalado las dos versiones de Visual Studio.
- **Se produce un error en la desinstalación de ASP.NET MVC 4 Si ya se han desinstalado los requisitos previos.** Para desinstalar correctamente ASP.NET MVC 4You debe desinstalar ASP.NET MVC 4 antes de desinstalar Visual Studio.
- **La instalación de ASP.NET MVC 4 interrumpe las aplicaciones de ASP.NET MVC 3 RTM.** Las aplicaciones ASP.NET MVC 3 que se crearon con la versión RTM (no con la versión de actualización de las [herramientas de ASP.NET MVC 3](https://www.microsoft.com/download/details.aspx?id=1491) ) requieren los siguientes cambios para funcionar en paralelo con ASP.NET MVC 4. Al compilar el proyecto sin que estas actualizaciones se realicen, se producirán errores de compilación. 

    **Actualizaciones necesarias**

  1. En el archivo raíz Web. config, agregue una nueva entrada *&lt;appSettings&gt;* con las *páginas web de clave: version* y el valor *1.0.0.0*. 

      [!code-xml[Main](mvc4-release-notes/samples/sample7.xml)]
  2. En Explorador de soluciones, haga clic con el botón secundario en el nombre del proyecto y seleccione descargar el proyecto. A continuación, vuelva a hacer clic con el botón derecho en el nombre y seleccione Editar *nombreDeProyecto*. csproj.
  3. Busque las siguientes referencias de ensamblado: 

      [!code-xml[Main](mvc4-release-notes/samples/sample8.xml)]

      Reemplácelos por lo siguiente:

      [!code-xml[Main](mvc4-release-notes/samples/sample9.xml)]
  4. Guarde los cambios, cierre el archivo de proyecto (. csproj) que estaba editando y, a continuación, haga clic con el botón derecho en el proyecto y seleccione volver a cargar.

- Al **cambiar un proyecto de ASP.NET MVC 4 para que tenga como destino 4,0 de 4,5, no se actualiza la referencia de ensamblado de EntityFramework:** Si cambia un proyecto de ASP.NET MVC 4 para que tenga como destino 4,0 después de destino 4,5, la referencia al ensamblado EntityFramework seguirá señalando a la versión 4,5. Para corregir este problema, desinstale y vuelva a instalar el paquete NuGet EntityFramework.
- **403 prohibido al ejecutar una aplicación de ASP.NET MVC 4 en Azure después de cambiar a 4,0 de destino de 4,5:** Si cambia un proyecto de ASP.NET MVC 4 para que tenga como destino 4,0 después de destino 4,5 y luego implementa en Azure, es posible que vea un error de la prohibición de 403 en tiempo de ejecución. Para solucionar este problema, agregue lo siguiente al archivo Web. config: `<modules runAllManagedModulesForAllRequests="true" />`
- **Visual Studio 2012 se bloquea cuando se escribe un "\' en un literal de cadena en un archivo de Razor.** Para solucionar el problema, escriba primero la comilla de cierre del literal de cadena.
- <strong>Al ir a &quot;cuenta/administrar&quot; en la plantilla de Internet se produce un error de tiempo de ejecución para los idiomas CHS, MCA y CHT.</strong> Para corregir el problema, modifique la página para separar <em>@User.Identity.Name</em> . para ello, colóquelo como el único contenido dentro de la etiqueta <em>&lt;Strong&gt;</em> .
- **Los proveedores de Google y LinkedIn no se admiten en sitios web de Azure.** Use proveedores de autenticación alternativos al implementar en sitios web de Azure.
- **Al usar UriPathExtensionMapping con IIS 8 Express/IIS, recibirá errores 404 no encontrados al intentar utilizar la extensión.** El controlador de archivos estáticos interferirá con las solicitudes a las API Web que usan *UriPathExtensionMappings*. Establezca *runAllManagedModulesForAllRequests = true* en el archivo Web. config para solucionar el problema.
- **Ya no se llama al método Controller. Execute.** Todos los controladores MVC siempre se ejecutan de forma asincrónica.
