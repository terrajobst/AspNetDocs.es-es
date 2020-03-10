---
uid: whitepapers/aspnet-and-web-tools-20122-release-notes
title: Notas de la versión de ASP.NET and Web Tools 2012,2 | Microsoft Docs
author: rick-anderson
description: Notas de la versión de ASP.NET and Web Tools 2012,2.
ms.author: riande
ms.date: 02/14/2013
ms.assetid: bdb18d02-9f61-4676-836d-6fdea94f9282
msc.legacyurl: /whitepapers/aspnet-and-web-tools-20122-release-notes
msc.type: content
ms.openlocfilehash: a4ea1d7c146309e1d5e8be944d496e9fd87bca3e
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78420253"
---
# <a name="aspnet-and-web-tools-20122-release-notes"></a>Notas de la versión de ASP.NET and Web Tools 2012.2

> En este documento se describe la versión de ASP.NET and Web Tools 2012,2. Es una actualización de las herramientas Web de Visual Studio y ASP.NET.

- [Notas de instalación](#_Installation)
- [Documentación](#_Documentation)
- [Compatibilidad](#_Support)
- [Requisitos de software](#_Software_Requirements)
- [Nuevas características de ASP.NET and Web Tools 2012,2](#_New_Features_in)

    - [Herramientas](#_Tooling)
    - [Publicación en Web](#_Web_Publishing)
    - [Plantillas MVC de ASP.NET](#_Templates)
    - [ASP.NET Web API](#_ASP.NET_Web_API) (Más información sobre ASP.NET Web API)

    - [ASP.NET SignalR](#_ASP.NET_SignalR)
    - [Direcciones URL descriptivas de ASP.NET](#_ASP.NET_Friendly_URLs)
- [Problemas conocidos y cambios importantes](#_Known_Issues_and)

<a id="_Installation"></a>
## <a name="installation-notes"></a>Notas de la instalación

ASP.NET and Web Tools 2012,2 para Visual Studio 2012 se puede instalar mediante el [instalador de plataforma web](https://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=ASPDOTNETandWebTools2012_2). Se trata de una actualización de Visual Studio 2012 o Visual Studio Express 2012 para Web, que es necesaria. Si no tiene instalado Visual Studio, se instalará Visual Studio Express 2012 para Web.

También puede instalar ASP.NET and Web Tools 2012,2 de forma manual. Debe tener instalado Visual Studio 2012 o Visual Studio Express 2012 para Web. A continuación, siga estas instrucciones: 

1. Descargue el instalador de [ASP.net y Web framework 2012,2](https://download.microsoft.com/download/6/5/6/6562AFBE-9503-4E64-970C-1427133FCD73/AspNetWebTools2012Setup.exe) desde el centro de descarga.
2. Cuando se le pida, haga clic en ejecutar. También puede guardar el archivo para ejecutarlo más adelante.
3. Compruebe la versión de Visual Studio que va a actualizar. Puede hacerlo iniciando el Visual Studio que desea actualizar. A continuación, haga clic en el elemento de menú ayuda.   
    ![](aspnet-and-web-tools-20122-release-notes/_static/image1.jpg)
4. Si ve el elemento de menú &quot;acerca de Microsoft Visual Studio 2012 para web&quot; descargue [web Herramientas de desarrollo 2012,2-Visual Studio Express 2012 para web](https://go.microsoft.com/fwlink/?LinkID=282228). De lo contrario, descargue [herramientas de desarrollo Web 2012,2-Visual Studio 2012](https://go.microsoft.com/fwlink/?LinkID=282228).
5. Cuando se le pida, haga clic en ejecutar. También puede guardar el archivo para ejecutarlo más adelante.

> [!NOTE]
> ASP.NET and Web Tools versión 2012,2 no incluye SQL Server Data Tools. SQL Server y las bases de datos SQL de Windows Azure proporcionan un conjunto más completo de herramientas de base de datos, entre las que se incluyen desarrollo sin conexión respaldado por proyectos, comparación de esquemas y capacidades de implementación de base de datos mejorada. Para obtener más información o para instalar SQL Server Data Tools visite [https://go.microsoft.com/fwlink/?LinkID=237127](https://go.microsoft.com/fwlink/?LinkID=237127).

<a id="_Documentation"></a>
## <a name="documentation"></a>Documentación

Puede encontrar tutoriales y otra información sobre ASP.NET and Web Tools 2012,2 en el sitio web de ASP.NET (https://www.asp.net).

<a id="_Support"></a>
## <a name="support"></a>Compatibilidad

ASP.NET and Web Tools 2012,2 está oficialmente disponible y es compatible. Puede usar el canal de soporte técnico normal. También puede publicar preguntas en los foros de ASP.NET ([https://forums.asp.net/](https://forums.asp.net/)), donde los miembros de la comunidad de ASP.net suelen ser capaces de proporcionar soporte técnico informal.

<a id="_Software_Requirements"></a>
## <a name="software-requirements"></a>Requisitos de software

El ASP.NET and Web Tools 2012,2 requiere Visual Studio 2012 o Visual Studio Express 2012 para Web.

<a id="_New_Features_in"></a>
## <a name="new-features-in-aspnet-and-web-tools-20122"></a>Nuevas características de ASP.NET and Web Tools 2012,2

En esta sección se describen las características que se han introducido en la versión ASP.NET and Web Tools 2012,2.

<a id="_Tooling"></a>
### <a name="tooling"></a>Tooling

- Inspector de página 

    - Compatibilidad con la asignación de selección de JavaScript que permite a Inspector de página asignar elementos que se agregaron dinámicamente a la página al código de JavaScript correspondiente.
    - La capacidad de ver las actualizaciones de CSS en tiempo real.
    - Para obtener más información, lea [sincronización automática de CSS y asignación de selección de JavaScript en inspector de página](https://blogs.msdn.com/b/webdev/archive/2012/12/14/css-auto-sync-and-javascript-selection-mapping-in-page-inspector.aspx).
- Editor 

    - Admite el resaltado de sintaxis para CoffeeScript, Mustache, manillar y JsRender.
    - El editor HTML proporciona IntelliSense para los enlaces de cobertura.
    - MENOS compatibilidad de edición y compilador para habilitar la creación de CSS dinámicas con menos.
    - Pegue JSON como una clase .NET. Con este comando especial de pegado para pegar JSON C# en un archivo de código o VB.net, y Visual Studio generará automáticamente clases de .net que se deducen a partir de JSON.
- La compatibilidad con el emulador móvil agrega enlaces de extensibilidad para que los emuladores de terceros puedan instalarse como VSIX. Los emuladores instalados se mostrarán en la lista desplegable F5 para que los desarrolladores puedan obtener una vista previa de sus sitios web en una variedad de dispositivos móviles. Obtenga más información sobre esta característica en la entrada de blog de Scott Hanselman en [la nueva integración de BrowserStack con Visual Studio](http://www.hanselman.com/blog/CrossBrowserDebuggingIntegratedIntoVisualStudioWithBrowserStack.aspx).

<a id="_Web_Publishing"></a>
### <a name="web-publishing"></a>publicación Web

- Los proyectos de sitio web ahora tienen la misma experiencia de publicación que los proyectos de aplicación Web, incluida la publicación en sitios web de Windows Azure.
- Publicación &#8211; selectiva para uno o varios archivos puede realizar las siguientes acciones (después de la publicación en un punto de conexión de web deploy): 

    - Publicar archivos seleccionados.
    - Vea la diferencia entre un archivo local y un archivo remoto.
    - Actualice el archivo local con el archivo remoto o actualice el archivo remoto con el archivo local.

<a id="_Templates"></a>
### <a name="aspnet-mvc-templates"></a>Plantillas MVC de ASP.NET

- La nueva plantilla de aplicación de Facebook facilita la escritura de aplicaciones Canvas de Facebook. En unos pocos pasos sencillos, puede crear una aplicación de Facebook que obtiene datos de un usuario conectado y se integra con los amigos de este. La plantilla incluye una nueva biblioteca para encargarse de todos los elementos necesarios a la hora de crear una aplicación de Facebook, incluida la autenticación, los permisos, el acceso a los datos de Facebook y mucho más. Para obtener más información sobre el uso de la plantilla de aplicación de Facebook, consulte [https://go.microsoft.com/fwlink/?LinkID=269921](https://go.microsoft.com/fwlink/?LinkID=269921).
- Una nueva plantilla de MVC de aplicación de página única permite a los desarrolladores crear aplicaciones web de cliente interactivas mediante HTML 5, CSS 3 y las populares bibliotecas Knockout y jQuery JavaScript, encima de ASP.NET Web API. La plantilla incluye una aplicación de lista "todo" que muestra las prácticas comunes para compilar una aplicación HTML5 de JavaScript que usa una API de servidor RESTful. Puede leer más en [https://www.asp.net/single-page-application](../single-page-application/index.md).
- Ahora puede crear un VSIX que agregue nuevas plantillas al cuadro de diálogo nuevo proyecto de ASP.NET MVC. Obtenga información sobre cómo hacerlo aquí: [https://go.microsoft.com/fwlink/?LinkId=275019](https://go.microsoft.com/fwlink/?LinkId=275019)
- Las plantillas &#8211; de proyecto de MVC del paquete FixedDisplayModes se han actualizado para incluir el nuevo paquete NuGet "FixedDisplayModes", que contiene una solución alternativa para un error en MVC 4. Para obtener más información sobre la corrección incluida en el paquete, consulte esta entrada de blog ([https://blogs.msdn.com/b/rickandy/archive/2012/09/17/asp-net-mvc-4-mobile-caching-bug-fixed.aspx](https://blogs.msdn.com/b/rickandy/archive/2012/09/17/asp-net-mvc-4-mobile-caching-bug-fixed.aspx)) del equipo de MVC.

<a id="_ASP.NET_Web_API"></a>
### <a name="aspnet-web-api"></a>ASP.NET Web API

ASP.NET Web API se ha mejorado con varias características nuevas:

- ASP.NET Web API OData
- Seguimiento de ASP.NET Web API
- ASP.NET Web API página de ayuda

#### <a name="aspnet-web-api-odata"></a>ASP.NET Web API OData

ASP.NET Web API OData le ofrece la flexibilidad que necesita para crear puntos de conexión de OData con lógica de negocios enriquecida a través de cualquier origen de datos. Con ASP.NET Web API OData puede controlar la cantidad de semántica de OData que desea exponer. ASP.NET Web API OData se incluye con las plantillas de proyecto de ASP.NET MVC 4 y también está disponible en NuGet ([http://www.nuget.org/packages/microsoft.aspnet.webapi.odata](http://www.nuget.org/packages/microsoft.aspnet.webapi.odata)).

ASP.NET Web API OData actualmente admite las siguientes características:

- Habilite la semántica de consulta de OData aplicando el atributo [Queryable].
- Validar fácilmente las consultas de OData y restringir el conjunto de opciones de consulta, operadores y funciones admitidos.
- El parámetro se enlaza a ODataQueryOptions directamente para obtener una representación de árbol de sintaxis abstracta de la consulta que se puede validar y aplicar a un IQueryable o IEnumerable.
- Habilite la paginación controlada por servicios y la generación de vínculos de página siguiente mediante la especificación de los límites de resultados en el atributo [consultable].
- Solicitar un recuento insertado del número total de recursos coincidentes mediante $inlinecount.
- Propagación de valores NULL.
- Operadores any/all de $filter.
- Inferir un Entity Data Model por Convención o personalizar explícitamente un modelo de forma similar a Entity Framework Code-First.
- Exponga los conjuntos de entidades derivando de EntitySetController.
- Convenciones simples y personalizables para exponer propiedades de navegación, manipular vínculos e implementar acciones de OData.
- Enrutamiento simplificado mediante el método de extensión MapODataRoute.
- Compatibilidad con el control de versiones mediante la exposición de varios modelos EDM.
- Exponga el documento de servicio y la $metadata para que pueda generar clientes (.NET, Windows Phone, tienda Windows, etc.) para su API Web.
- Compatibilidad con los formatos de OData, JSON y JSON de OData.
- Cree, actualice, actualice parcialmente (PATCH) y elimine entidades.
- Consultar y manipular relaciones entre entidades.
- Cree vínculos de relación que se conecten a las rutas.
- Tipos complejos.
- Herencia de tipo de entidad.
- Propiedades de la colección.
- Enumeraciones.
- Acciones de OData.
- Se basa en la misma base que WCF Data Services, es decir, ODataLib ([http://www.nuget.org/packages/microsoft.data.odata](http://www.nuget.org/packages/microsoft.data.odata)).

Para obtener más información sobre ASP.NET Web API OData, consulte [https://go.microsoft.com/fwlink/?LinkId=271141](https://go.microsoft.com/fwlink/?LinkId=271141).

#### <a name="aspnet-web-api-tracing"></a>Seguimiento de ASP.NET Web API

El seguimiento de ASP.NET Web API integra datos de seguimiento de las API Web con seguimiento de .NET. Ahora está habilitada de forma predeterminada en la plantilla de proyecto de Web API. Los datos de seguimiento de las API Web se envían a la ventana de salida y se ponen a disposición a través de IntelliTrace. ASP.NET Web API Tracing le permite realizar un seguimiento de la información sobre la API Web cuando se hospeda en Windows Azure a través de la integración con [windows Azure Diagnostics](https://msdn.microsoft.com/library/windowsazure/hh411529.aspx). También puede instalar y habilitar el seguimiento de ASP.NET Web API en cualquier aplicación mediante el paquete NuGet de seguimiento de ASP.NET Web API ([http://www.nuget.org/packages/microsoft.aspnet.webapi.tracing](http://www.nuget.org/packages/microsoft.aspnet.webapi.tracing)).

Para obtener más información acerca de la configuración y el uso del seguimiento de ASP.NET Web API consulte [https://go.microsoft.com/fwlink/?LinkID=269874](https://go.microsoft.com/fwlink/?LinkID=269874).

#### <a name="aspnet-web-api-help-page"></a>ASP.NET Web API página de ayuda

La página de ayuda de ASP.NET Web API se incluye ahora de forma predeterminada en la plantilla de proyecto de API Web. En la página de ayuda de ASP.NET Web API se genera automáticamente documentación para las API Web, incluidos los puntos de conexión HTTP, los métodos HTTP admitidos, los parámetros y las cargas de mensajes de solicitud y respuesta de ejemplo. La documentación se extrae automáticamente de los comentarios del código. También puede Agregar la página de ayuda de ASP.NET Web API a cualquier aplicación mediante el paquete NuGet de la página de ayuda de ASP.NET Web API ([http://www.nuget.org/packages/microsoft.aspnet.webapi.helppage](http://www.nuget.org/packages/microsoft.aspnet.webapi.helppage)).

Para obtener más información sobre cómo configurar y personalizar la página de ayuda de ASP.NET Web API, vea [https://go.microsoft.com/fwlink/?LinkId=271140](https://go.microsoft.com/fwlink/?LinkId=271140).

<a id="_ASP.NET_SignalR"></a>
### <a name="aspnet-signalr"></a>ASP.NET SignalR

ASP.NET Signalr facilita la incorporación de funcionalidades Web en tiempo real a la aplicación de ASP.NET, mediante WebSockets, si está disponible y, de forma automática, se revierte a otras técnicas cuando no lo está.

Para obtener más información sobre el uso de ASP.NET Signalr, consulte [https://go.microsoft.com/fwlink/?LinkId=271271](https://go.microsoft.com/fwlink/?LinkId=271271).

<a id="_ASP.NET_Friendly_URLs"></a>
### <a name="aspnet-friendly-urls"></a>ASP.NET Friendly URLs

ASP.NET FriendlyURLs facilita enormemente que los desarrolladores de formularios Web Forms generen direcciones URL de búsqueda más nítidas (sin la extensión. aspx). Requiere poco o ninguna configuración y se puede usar con las aplicaciones existentes de ASP.NET v 4.0. La característica FriendlyURLs también facilita a los desarrolladores la incorporación de compatibilidad para dispositivos móviles a sus aplicaciones, ya que admite el cambio entre las vistas de escritorio y móviles.

Para obtener más información sobre la instalación y el uso de direcciones URL descriptivas de ASP.NET, vea [http://www.hanselman.com/blog/IntroducingASPNETFriendlyUrlsCleanerURLsEasierRoutingAndMobileViewsForASPNETWebForms.aspx](http://www.hanselman.com/blog/IntroducingASPNETFriendlyUrlsCleanerURLsEasierRoutingAndMobileViewsForASPNETWebForms.aspx).

<a id="_Known_Issues_and"></a>
## <a name="known-issues-and-breaking-changes"></a>Problemas conocidos y cambios importantes

En esta sección se describen los problemas conocidos y los cambios importantes que se encuentran en la versión ASP.NET and Web Tools 2012,2.

### <a name="installation-issues"></a>Problemas de instalación

#### <a name="out-of-order-installs-of-visual-studio-2012"></a>Instalaciones desordenadas de Visual Studio 2012

La instalación de una SKU adicional de Visual Studio 2012 después de instalar el ASP.NET and Web Tools 2012,2 requerirá una operación de reparación. Considere la siguiente secuencia:

1. Instalación de Visual Studio 2012 Express para Web
2. Instalación de ASP.NET and Web Tools 2012,2
3. Instalación de Visual Studio 2012 Professional, Premium o Ultimate

El paso 2 solo daría lugar a la instalación de actualizaciones de Express for Web. Para asegurarse de que la SKU adicional instalada durante el paso 3 contiene la actualización, deberá reparar el ASP.NET and Web Tools 2012,2 para instalar las actualizaciones de la última SKU instalada. Esto también se aplica si se invierten las SKU del paso 1 y 3.

#### <a name="installing-microsoft-aspnet-and-web-tools-20122-when-visual-studio-is-open"></a>Instalación de Microsoft ASP.NET and Web Tools 2012,2 cuando Visual Studio está abierto

Si VS está abierto durante la instalación de Microsoft ASP.NET and Web Tools 2012,2, Visual Studio podría acabar en un estado incorrecto. Se recomienda que los usuarios cierren todas las instancias de Visual Studio antes de continuar con la instalación.

#### <a name="canceling-aspnet-and-web-tools-20122-setup-in-the-middle-of-installation"></a>Cancelar ASP.NET and Web Tools instalación de 2012,2 en mitad de la instalación

Al cancelar ASP.NET and Web Tools instalación 2012,2 en medio de la instalación, se dejará Visual Studio en un estado incorrecto. Para solucionar este problema, siga estos pasos: 

- Vaya a Agregar o quitar programas.
- Desinstale Microsoft ASP.NET and Web Tools 2012,2, si está presente.
- Reinstalar Microsoft ASP.NET and Web Tools 2012,2

#### <a name="after-uninstalling-aspnet-and-web-tools-20122-the-aspnet-mvc-4-templates-and-razor-v2-web-site-templates-are-missing"></a>Después de desinstalar ASP.NET and Web Tools 2012,2 faltan las plantillas de ASP.NET MVC 4 y las plantillas de sitio web de Razor V2

Al desinstalar ASP.NET and Web Tools 2012,2 también se desinstalarán todas las plantillas de sitio web de ASP.NET MVC 4 y Razor V2 de Visual Studio 2012.

La solución alternativa es reparar la instalación de Visual Studio 2012 para reinstalar las plantillas de sitio web de ASP.NET MVC 4 y Razor V2.

### <a name="tooling-issues"></a>Problemas con las herramientas

#### <a name="nuget-error-reported-during-project-creation"></a>Error de NuGet detectado durante la creación del proyecto

Después de instalar ASP.NET and Web Tools 2012,2, es posible que vea el siguiente error al crear un proyecto de MVC 4.

![](aspnet-and-web-tools-20122-release-notes/_static/image1.png)

El ASP.NET and Web Tools 2012,2 envía NuGet 2,1 y actualizará la extensión en Visual Studio 2012. En algunos casos, el instalador de VSIX no podrá actualizar correctamente el VSIX. Los pasos siguientes le permitirán solucionar este problema:

1. Inicie Visual Studio 2012 como administrador
2. Vaya a herramientas-&gt;extensiones y actualizaciones y desinstale NuGet.
3. Cierre Visual Studio.
4. Navegue hasta la carpeta de instalación de ASP.NET and Web Tools 2012,2:

    1. Para Visual Studio 2012: **archivos de programa\Microsoft ASP. NET\ASP.net web Stack\Visual Studio 2012**
    2. Para Visual Studio 2012 Express para Web: **archivos de programa\Microsoft ASP. NET\ASP.net web Stack\Visual Studio Express 2012 for Web**
5. Haga doble clic en NuGet. Tools. VSIX para reinstalar NuGet.

### <a name="web-api-issues"></a>Problemas de la API Web

#### <a name="parsing-issues-in-filter-and-datetime-literals"></a>Problemas de análisis en los literales $filter y DateTime

El analizador de URI de OData no analiza correctamente los literales de fecha y hora parciales. Por ejemplo, $filter = Start EQ DateTime ' 2012-12-31T12:00 ' no se puede analizar correctamente. Una solución alternativa es usar el literal completo, $filter = Start EQ DateTime ' 2012-12-31T12:00:00 '.

#### <a name="odata-doesnt-support-case-insensitive-property-names"></a>OData no admite nombres de propiedad que no distinguen mayúsculas de minúsculas.

OData no admite nombres de propiedad que no distinguen mayúsculas de minúsculas en las consultas de OData y la ruta de acceso de oData. Vea elementos de trabajo:

- [http://aspnetwebstack.codeplex.com/workitem/366](http://aspnetwebstack.codeplex.com/workitem/366)
- [http://aspnetwebstack.codeplex.com/workitem/704](http://aspnetwebstack.codeplex.com/workitem/704)

Si los usuarios tienen distintas mayúsculas y minúsculas en el lado cliente y en el servidor de JavaScript, probablemente se producirá este problema. Este problema se debe al diseño en el protocolo OData. Sin embargo, muchos usuarios informan de este problema. Para solucionarlo, los usuarios tienen que corregir los casos en la dirección URL.

#### <a name="default-odata-routing-conventions-doesnt-support-postput-on-navigation-property"></a>Las convenciones de enrutamiento de OData predeterminadas no admiten POST/PUT en la propiedad de navegación.

Las convenciones de enrutamiento de OData predeterminadas no admiten POST/PUT en la propiedad de navegación. Vea [http://aspnetwebstack.codeplex.com/workitem/366](http://aspnetwebstack.codeplex.com/workitem/366)de trabajo. Falta esta Convención de uso común en las convenciones predeterminadas.

Para solucionarlo, los usuarios deben ampliar la nueva Convención de enrutamiento para que sea compatible.

### <a name="facebook-template-issues"></a>Problemas de la plantilla de Facebook

#### <a name="facebook-application-template-only-works-using-net-45"></a>La plantilla de aplicación de Facebook solo funciona con .NET 4,5

Debe seleccionar .NET 4,5 en la lista desplegable de Framework en el cuadro de diálogo nuevo proyecto para ver la plantilla de aplicación de Facebook en ASP.NET MVC 4.

#### <a name="real-time-update-controller"></a>Controlador de actualización en tiempo real

La plantilla de aplicación de Facebook permite al usuario crear fácilmente un controlador de API Web para controlar las actualizaciones en tiempo real de Facebook. Si el equipo de desarrollo está detrás de NAT, es posible que el controlador no funcione sin más configuración de red. Vea aquí para obtener más información: [http://facebook.stackoverflow.com/questions/5259467/can-a-computer-behind-a-nat-router-receive-realtime-updates-from-facebook](http://facebook.stackoverflow.com/questions/5259467/can-a-computer-behind-a-nat-router-receive-realtime-updates-from-facebook)

#### <a name="query-string-values-conflict-with-facebook-oauth-parameters"></a>Los valores de cadena de consulta están en conflicto con los parámetros OAuth de Facebook

Los campos siguientes entran en conflicto con la URL de devolución de llamada del cuadro de diálogo de OAuth de Facebook. No agregue sus propios valores de cadena de consulta con los siguientes nombres: código, error, error\_Descripción, error\_motivo.

#### <a name="using-page-inspector-with-facebook-template"></a>Uso de Inspector de página con la plantilla de Facebook

No se puede usar la característica Inspector de página de Visual Studio 2012 durante la depuración de la aplicación de Facebook. La Inspector de página no admite actualmente iframes.

### <a name="single-page-application-template-issues"></a>Problemas con plantillas de aplicación de una sola página

#### <a name="with-jquery-19knockout-221-update-when-running-default-mvc-spa-project-new-todo-item-edit-enter-focus-event-is-not-handled-properly"></a>Con JQuery 1.9/Knockout 2.2.1 Update, al ejecutar el proyecto de MVC SPA predeterminado, no se controla correctamente el evento de selección de edición de nuevo elemento de la opción Enter Focus.

Con JQuery 1.9/Knockout 2.2.1 Update, al ejecutar el proyecto de MVC SPA predeterminado, la nueva edición de elementos de la lista de tareas no volverá a centrarse en el cuadro de edición nuevo elemento de tarea después de escribir el nuevo elemento todo en la lista de tareas pendientes.

En la [http://knockoutjs.com/documentation/hasfocus-binding.html](http://knockoutjs.com/documentation/hasfocus-binding.html)de referencia de la solución alternativa y realice una corrección similar al código de ejemplo siguiente:

Archivo todo. Model. js  
 función ToDoList (Data), agregue lo siguiente:  
 **Self. isSelected = Ko. observable (false);**

función todoList. prototype. addTodo, agregue el siguiente texto en negro:  
 **Self. isSelected (true);**  
 Self. newTodoTitle (&quot;&quot;);

File index. cshtml, agregue el siguiente texto en negro:  
 &lt;form data-BIND =&quot;Submit: addTodo&quot;&gt;  
 &lt;clase de entrada =&quot;addTodo&quot; tipo =&quot;texto&quot; enlace de datos =&quot;valor: newTodoTitle, marcador de posición: ' Escriba aquí para agregar ', blurOnEnter: true, **hasFocus: IsSelected**, evento: {Blur: addTodo}&quot; /&gt;  
 &lt;/Form.&gt;
