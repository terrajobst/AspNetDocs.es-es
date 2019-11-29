---
uid: visual-studio/overview/2013/release-notes
title: ASP.NET and Web Tools para las notas de la versión de Visual Studio 2013 | Microsoft Docs
author: microsoft
description: En este documento se describe la versión de ASP.NET and Web Tools para Visual Studio 2013.
ms.author: riande
ms.date: 10/17/2013
ms.assetid: 08815768-2702-42ae-ae85-0a59934a11d1
msc.legacyurl: /visual-studio/overview/2013/release-notes
msc.type: authoredcontent
ms.openlocfilehash: d8af9c8e7ee1316a5eac90c5959d07c628154e09
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/28/2019
ms.locfileid: "74600435"
---
# <a name="aspnet-and-web-tools-for-visual-studio-2013-release-notes"></a>Notas de la versión de ASP.NET and Web Tools para Visual Studio 2013

por [Microsoft](https://github.com/microsoft)

> En este documento se describe la versión de ASP.NET and Web Tools para Visual Studio 2013.

## <a name="contents"></a>Contenido

- [Notas de instalación](#TOC1)
- [Documentación](#TOC2)
- [Requisitos de software](#TOC4)

### <a name="new-features-in-aspnet-and-web-tools-for-visual-studio-2013"></a>Nuevas características de ASP.NET and Web Tools para Visual Studio 2013

- [Una ASP.NET](#TOC6)
- [Nueva experiencia de proyecto web](#newproj)
- [Scaffolding de ASP.NET](#scaffold)
- [Vínculo con exploradores](#browser-link)
- [Mejoras del editor web de Visual Studio](#web-editor)
- [Compatibilidad de Azure App Service Web Apps en Visual Studio](#waws)
- [Mejoras en la publicación en Web](#publish)
- [NuGet 2.7](#nuget)
- [Formularios Web Forms ASP.NET](#TOC9)
- [ASP.NET MVC 5](#TOC10)
- [ASP.NET Web API 2](#TOC11)
- [ASP.NET Signalr](#TOC13)
- [ASP.NET Identity](#TOC8)
- [Componentes OWIN de Microsoft](#TOC7)
- [Entity Framework 6](#ef6)
- [ASP.NET Razor 3](#TOC14)
- [Suspensión de la aplicación ASP.NET](#TOC15)
- [Problemas conocidos y cambios importantes](#knownissues)

<a id="TOC1"></a>
## <a name="installation-notes"></a>Notas de la instalación

ASP.NET and Web Tools para Visual Studio 2013 se incluyen en el instalador principal y se pueden descargar [aquí](https://www.asp.net/downloads).

<a id="TOC2"></a>
## <a name="documentation"></a>Documentation

Los tutoriales y otra información acerca de ASP.NET and Web Tools para Visual Studio 2013 están disponibles en el [sitio web de ASP.net](https://www.asp.net/).

<a id="TOC4"></a>
## <a name="software-requirements"></a>Requisitos de software

ASP.NET and Web Tools requiere Visual Studio 2013.

<a id="TOC5"></a>
## <a name="new-features-in-aspnet-and-web-tools-for-visual-studio-2013"></a>Nuevas características de ASP.NET and Web Tools para Visual Studio 2013

En las secciones siguientes se describen las características que se han introducido en la versión.

<a id="TOC6"></a>
## <a name="one-aspnet"></a>Una ASP.NET

Con el lanzamiento de Visual Studio 2013, hemos realizado un paso para unificar la experiencia de uso de las tecnologías de ASP.NET, de modo que pueda mezclar y encontrar fácilmente las que quiera. Por ejemplo, puede iniciar un proyecto con MVC y agregar fácilmente páginas de formularios Web Forms al proyecto más adelante o las API Web de scaffolding en un proyecto de formularios Web Forms. Una ASP.NET es hacer que sea más fácil como desarrollador hacer lo que le encanta en ASP.NET. Independientemente de la tecnología que elija, puede tener la confianza de que está creando en el marco subyacente de confianza de un ASP.NET.

<a id="newproj"></a>
## <a name="new-web-project-experience"></a>Nueva experiencia de proyecto web

Hemos mejorado la experiencia de creación de nuevos proyectos web en Visual Studio 2013. En el cuadro de diálogo **nuevo proyecto Web ASP.net** puede seleccionar el tipo de proyecto que desee, configurar cualquier combinación de tecnologías (formularios Web Forms, MVC, API Web), configurar opciones de autenticación y agregar un proyecto de prueba unitaria.

![Nuevo proyecto de ASP.NET](release-notes/_static/image1.png)

El nuevo cuadro de diálogo permite cambiar las opciones de autenticación predeterminadas para muchas de las plantillas. Por ejemplo, al crear un proyecto de formularios Web Forms de ASP.NET, puede seleccionar cualquiera de las siguientes opciones:

- Sin autenticación
- Cuentas de usuario individuales (inicio de sesión de ASP.NET o de proveedor de redes sociales)
- Cuentas organizativas (Active Directory en una aplicación de Internet)
- Autenticación de Windows (Active Directory en una aplicación de intranet)

![Opciones de autenticación](release-notes/_static/image2.png)

Para obtener más información sobre el nuevo proceso de creación de proyectos Web, vea [crear proyectos Web de ASP.net en Visual Studio 2013](creating-web-projects-in-visual-studio.md). Para obtener más información acerca de las nuevas opciones de autenticación, vea [ASP.net Identity](#TOC8) más adelante en este documento.

<a id="scaffold"></a>
## <a name="aspnet-scaffolding"></a>Scaffolding de ASP.NET

ASP.NET scaffolding es un marco de generación de código para aplicaciones Web de ASP.NET. Facilita la adición de código reutilizable al proyecto que interactúa con un modelo de datos.

En versiones anteriores de Visual Studio, el scaffolding estaba limitado a los proyectos de MVC de ASP.NET. Con Visual Studio 2013, ahora puede usar la técnica scaffolding para cualquier proyecto de ASP.NET, incluidos los formularios Web Forms. En la actualidad, Visual Studio 2013 no admite la generación de páginas para un proyecto de formularios Web Forms, pero puede seguir usando la técnica scaffolding con formularios Web Forms agregando dependencias de MVC al proyecto. La compatibilidad con la generación de páginas para formularios Web Forms se agregará en una actualización futura.

Al usar la técnica scaffolding, garantizamos que todas las dependencias necesarias están instaladas en el proyecto. Por ejemplo, si comienza con un proyecto de formularios Web Forms de ASP.NET y, a continuación, usa scaffolding para agregar un controlador de API Web, los paquetes y las referencias de NuGet necesarios se agregan automáticamente al proyecto.

Para agregar scaffolding de MVC a un proyecto de formularios Web Forms, agregue un **nuevo elemento con scaffolding** y seleccione **dependencias de MVC 5** en la ventana del cuadro de diálogo. Hay dos opciones para la scaffolding de MVC; Mínima y completa. Si selecciona mínima, solo se agregan al proyecto los paquetes NuGet y las referencias para ASP.NET MVC. Si selecciona la opción completa, se agregan las dependencias mínimas, así como los archivos de contenido necesarios para un proyecto de MVC.

La compatibilidad con los controladores asincrónicos de scaffolding usa las nuevas características de Async de Entity Framework 6.

Para obtener más información y tutoriales, vea [información general sobre scaffolding de ASP.net](aspnet-scaffolding-overview.md).

<a id="browser-link"></a>
## <a name="browser-link--signalr-channel-between-browser-and-visual-studio"></a>Vínculo de explorador: canal de Signalr entre el explorador y Visual Studio

La nueva característica de [vínculo del explorador](using-browser-link.md) permite conectar varios exploradores a Visual Studio y actualizarlos haciendo clic en un botón de la barra de herramientas. Puede conectar varios exploradores al sitio de desarrollo, incluidos los emuladores móviles, y hacer clic en actualizar para actualizar todos los exploradores al mismo tiempo. El vínculo del explorador también expone una API que permite a los desarrolladores escribir extensiones de vínculo del explorador.

![](release-notes/_static/image3.png)

Al permitir que los desarrolladores aprovechen la API de vínculo del explorador, se pueden crear escenarios muy avanzados que crucen los límites entre Visual Studio y cualquier explorador que esté conectado. Web Essentials saca partido de la API para crear una experiencia integrada entre Visual Studio y las herramientas de desarrollo del explorador, el control remoto de los emuladores móviles y mucho más.

<a id="web-editor"></a>
## <a name="visual-studio-web-editor-enhancements"></a>Mejoras del editor web de Visual Studio

Visual Studio 2013 incluye un nuevo editor HTML para archivos de Razor y HTML en aplicaciones Web. El nuevo editor HTML proporciona un único esquema unificado basado en HTML5. Tiene la finalización automática de llaves, la interfaz de usuario de jQuery y el atributo de AngularJS IntelliSense, la agrupación de IntelliSense de atributo, el identificador y el nombre de clase IntelliSense y otras mejoras, como un mejor rendimiento, formato y SmartTags.

En la captura de pantalla siguiente se muestra el uso de IntelliSense de atributo bootstrap en el editor HTML.

![IntelliSense en el editor HTML](release-notes/_static/image4.png)

Visual Studio 2013 también incluye los editores CoffeeScript y LESS integrados. El editor LESS incluye todas las características interesantes del editor CSS y tiene IntelliSense específico para variables y mixins en todos los documentos menos de la cadena de @import.

<a id="waws"></a>
## <a name="azure-app-service-web-apps-support-in-visual-studio"></a>Compatibilidad de Azure App Service Web Apps en Visual Studio

En Visual Studio 2013 con el SDK de Azure para .NET 2,2, puede usar **Explorador de servidores** para interactuar directamente con las aplicaciones web remotas. Puede iniciar sesión en su cuenta de Azure, crear nuevas aplicaciones Web, configurar aplicaciones, ver registros en tiempo real y mucho más. Próximamente después del lanzamiento del SDK 2,2, podrá ejecutar en modo de depuración de forma remota en Azure. La mayoría de las nuevas características de Azure App Service Web Apps también funcionan en Visual Studio 2012 al instalar la versión actual del SDK de Azure para .NET.

Para obtener más información, vea los siguientes recursos:

- [Creación de una aplicación Web de ASP.NET en Azure App Service](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-get-started/)
- [Solución de problemas de una aplicación web en Azure App Service con Visual Studio](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/)

<a id="publish"></a>
## <a name="web-publish-enhancements"></a>Mejoras en la publicación en Web

Visual Studio 2013 incluye características de publicación web nuevas y mejoradas. Estos son algunos de ellos:

- [Automatice fácilmente el cifrado de archivos Web. config](https://go.microsoft.com/fwlink/?LinkId=325529). (Este vínculo y los dos apuntan a la documentación de MSDN que podrían no estar disponibles hasta el día 10/17).
- [Automatizar fácilmente el uso de una aplicación durante la implementación](https://go.microsoft.com/fwlink/?LinkId=325530).
- Configure Web Deploy para [usar la suma de comprobación de archivo en lugar de la fecha de última modificación](https://go.microsoft.com/fwlink/?LinkId=325531) para determinar qué archivos deben copiarse en el servidor.
- Publique rápidamente archivos seleccionados individuales (incluido Web. config) cuando use los métodos de publicación FTP o del sistema de archivos, así como con Web Deploy.

Para obtener más información acerca de la implementación web de ASP.NET, consulte [el sitio de ASP.net](https://go.microsoft.com/fwlink/?LinkId=322027).

<a id="nuget"></a>
## <a name="nuget-27"></a>NuGet 2.7

NuGet 2,7 incluye un amplio conjunto de nuevas características que se describen en detalle en las notas de la [versión de nuget 2,7](http://docs.nuget.org/docs/release-notes/nuget-2.7).

Esta versión de NuGet también elimina la necesidad de proporcionar consentimiento explícito para que la característica de restauración de paquetes de NuGet Descargue paquetes. El consentimiento (y la casilla asociado en el cuadro de diálogo Preferencias de NuGet) ahora se concede mediante la instalación de NuGet. Ahora, la restauración de paquetes simplemente funciona de forma predeterminada.

<a id="TOC9"></a>
## <a name="aspnet-web-forms"></a>Formularios Web Forms de ASP.NET

### <a name="one-aspnet"></a>Una ASP.NET

Las plantillas de proyecto de formularios Web Forms se integran perfectamente con la nueva experiencia de ASP.NET. Puede Agregar compatibilidad con MVC y API Web a su proyecto de formularios Web Forms, y puede configurar la autenticación mediante el Asistente para crear un proyecto de ASP.NET. Para obtener más información, vea [crear proyectos Web de ASP.net en Visual Studio 2013](creating-web-projects-in-visual-studio.md).

### <a name="aspnet-identity"></a>ASP.NET Identity

Las plantillas de proyecto de formularios Web Forms admiten el nuevo marco de ASP.NET Identity. Además, las plantillas ahora admiten la creación de un proyecto de intranet de formularios Web Forms. Para obtener más información, vea [métodos de autenticación](creating-web-projects-in-visual-studio.md#auth) en **crear proyectos Web de ASP.net en Visual Studio 2013**.

### <a name="bootstrap"></a>Bootstrap

Las plantillas de formularios Web Forms usan [bootstrap](http://twitter.github.io/bootstrap/) para proporcionar una apariencia elegante y con capacidad de respuesta que se puede personalizar fácilmente. Para obtener más información, consulte [bootstrap en el Visual Studio 2013 plantillas de proyecto web](creating-web-projects-in-visual-studio.md#bootstrap).

<a id="TOC10"></a>
## <a name="aspnet-mvc-5"></a>ASP.NET MVC 5

### <a name="one-aspnet"></a>Una ASP.NET

Las plantillas de proyecto de web MVC se integran perfectamente con la nueva experiencia de ASP.NET. Puede personalizar el proyecto de MVC y configurar la autenticación mediante el Asistente para crear un proyecto de ASP.NET. Puede encontrar un tutorial introductorio para ASP.NET MVC 5 en [Introducción con ASP.NET MVC 5](../../../mvc/overview/getting-started/introduction/getting-started.md).

Para obtener información sobre cómo actualizar proyectos de MVC 4 a MVC 5, vea [Cómo actualizar un proyecto de ASP.NET MVC 4 y Web API a ASP.NET MVC 5 y Web API 2](../../../mvc/overview/releases/how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2.md).

### <a name="aspnet-identity"></a>ASP.NET Identity

Las plantillas de proyecto de MVC se han actualizado para usar ASP.NET Identity para la autenticación y la administración de identidades. Encontrará un tutorial que incluye la autenticación de Facebook y Google y la nueva API de pertenencia en [creación de una aplicación de ASP.NET MVC 5 con el inicio de sesión de Facebook y Google OAuth2 y OpenID,](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) y [creación de una aplicación de ASP.NET MVC con autenticación y base de SQL Database e implementación en Azure App Service](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/).

### <a name="bootstrap"></a>Bootstrap

La plantilla de proyecto de MVC se ha actualizado para usar [bootstrap](http://getbootstrap.com/) para proporcionar una apariencia elegante y con capacidad de respuesta que se puede personalizar fácilmente. Para obtener más información, consulte [bootstrap en el Visual Studio 2013 plantillas de proyecto web](creating-web-projects-in-visual-studio.md#bootstrap).

### <a name="authentication-filters"></a>Filtros de autenticación

Los filtros de autenticación son un nuevo tipo de filtro en ASP.NET MVC que se ejecuta antes de los filtros de autorización en la canalización ASP.NET MVC y le permite especificar la lógica de autenticación por acción, por controlador o globalmente para todos los controladores. Los filtros de autenticación procesan las credenciales en la solicitud y proporcionan una entidad de seguridad correspondiente. Los filtros de autenticación también pueden agregar desafíos de autenticación en respuesta a solicitudes no autorizadas.

### <a name="filter-overrides"></a>Filtrar invalidaciones

Ahora puede invalidar los filtros que se aplican a un determinado método de acción o controlador especificando un filtro de invalidación. Los filtros de invalidación especifican un conjunto de tipos de filtro que no se deben ejecutar para un ámbito determinado (acción o controlador). Esto le permite configurar filtros que se aplican globalmente pero, a continuación, excluir determinados filtros globales de la aplicación de acciones o controladores específicos.

### <a name="attribute-routing"></a>Enrutamiento mediante atributos

ASP.NET MVC admite ahora el enrutamiento de atributos, gracias a la contribución de Tim McCall, el autor de [http://attributerouting.net](http://attributerouting.net). Con el enrutamiento de atributo puede especificar las rutas anotando las acciones y los controladores.

<a id="TOC11"></a>
## <a name="aspnet-web-api-2"></a>ASP.NET Web API 2

### <a name="attribute-routing"></a>Enrutamiento mediante atributos

ASP.NET Web API admite ahora el enrutamiento de atributos, gracias a la contribución de Tim McCall, el autor de [http://attributerouting.net](http://attributerouting.net). Con el enrutamiento de atributos, puede especificar las rutas de la API Web mediante la anotación de las acciones y los controladores como se indica a continuación:

[!code-csharp[Main](release-notes/samples/sample1.cs)]

El enrutamiento de atributos le proporciona más control sobre los URI de la API Web. Por ejemplo, puede definir fácilmente una jerarquía de recursos con un solo controlador de API:

[!code-csharp[Main](release-notes/samples/sample2.cs)]

El enrutamiento de atributos también proporciona una sintaxis adecuada para especificar parámetros opcionales, valores predeterminados y restricciones de ruta:

[!code-csharp[Main](release-notes/samples/sample3.cs)]

Para obtener más información sobre el enrutamiento de atributos, consulte [enrutamiento de atributos en Web API 2](../../../web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2.md).

### <a name="oauth-20"></a>OAuth 2,0

Las plantillas de proyecto de aplicación de página única y API Web ahora admiten la autorización mediante OAuth 2,0. OAuth 2,0 es un marco para autorizar el acceso de cliente a los recursos protegidos. Funciona para una gran variedad de clientes, incluidos exploradores y dispositivos móviles.

La compatibilidad con OAuth 2,0 se basa en el nuevo middleware de seguridad que proporcionan los componentes OWIN de Microsoft para la autenticación del portador y la implementación del rol de servidor de autorización. Como alternativa, los clientes pueden ser autorizados mediante un servidor de autorización de la organización, como Azure Active Directory o ADFS en Windows Server 2012 R2.

### <a name="odata-improvements"></a>Mejoras de OData

**Compatibilidad con $select, $expand, $batch y $value**

ASP.NET Web API OData ahora tiene compatibilidad total con $select, $expand y $value. También puede utilizar $batch para el procesamiento por lotes de solicitudes y el procesamiento de conjuntos de cambios.

Las opciones $select y $expand permiten cambiar la forma de los datos que se devuelven desde un punto de conexión de OData. Para obtener más información, consulte [introducing $Select and $Expand support in Web API OData](../../../web-api/overview/odata-support-in-aspnet-web-api/using-select-expand-and-value.md).

**Extensibilidad mejorada**

Los formateadores de OData ahora son extensibles. Puede agregar metadatos de entrada de Atom, admitir entradas de vínculo multimedia y de flujo con nombre, agregar anotaciones de instancia y personalizar el modo en que se generan los vínculos.

**Compatibilidad con tipos menos**

Ahora puede crear servicios de OData sin necesidad de definir tipos CLR para los tipos de entidad. En su lugar, los controladores de OData pueden tomar o devolver instancias de **IEdmObject**, que son los formateadores de oData Serialize/Deserialize.

**Reutilizar un modelo existente**

Si ya tiene un Entity Data Model (EDM) existente, ahora puede reutilizarlo directamente, en lugar de tener que crear uno nuevo. Por ejemplo, si usa Entity Framework, puede usar el EDM que EF crea automáticamente.

### <a name="request-batching"></a>Procesamiento por lotes de solicitudes

El procesamiento por lotes de solicitudes combina varias operaciones en una única solicitud HTTP POST, para reducir el tráfico de red y proporcionar una interfaz de usuario más sencilla y menos conversante. ASP.NET Web API admite ahora varias estrategias para el procesamiento por lotes de solicitudes:

- Use el punto de conexión $batch de un servicio de OData.
- Empaquete varias solicitudes en una única solicitud de varias partes MIME.
- Use un formato de procesamiento por lotes personalizado.

Para habilitar el procesamiento por lotes de solicitudes, solo tiene que agregar una ruta con un controlador de procesamiento por lotes a la configuración de la API Web:

[!code-csharp[Main](release-notes/samples/sample4.cs)]

También puede controlar si las solicitudes se ejecutan secuencialmente o en cualquier orden.

### <a name="portable-aspnet-web-api-client"></a>Cliente de ASP.NET Web API portátil

Ahora puede usar el cliente de ASP.NET Web API para crear bibliotecas de clases portables que funcionan en las aplicaciones de la tienda Windows y de Windows Phone 8. También puede crear formateadores portátiles que se pueden compartir entre el cliente y el servidor.

### <a name="improved-testability"></a>Mejora de la capacidad de prueba

Web API 2 hace que sea mucho más fácil realizar pruebas unitarias de los controladores de API. Simplemente cree una instancia del controlador de API con el mensaje de solicitud y la configuración y, después, llame al método de acción que desea probar. También es fácil simular la clase **UrlHelper** para los métodos de acción que realizan la generación de vínculos.

### <a name="ihttpactionresult"></a>IHttpActionResult

Ahora puede implementar IHttpActionResult para encapsular el resultado de los métodos de acción de la API Web. El tiempo de ejecución de ASP.NET Web API ejecuta un IHttpActionResult devuelto desde un método de acción de la API Web para generar el mensaje de respuesta resultante. Se puede devolver un IHttpActionResult desde cualquier acción de la API Web para simplificar las pruebas unitarias de la implementación de la API Web. Para mayor comodidad, se proporcionan una serie de implementaciones de IHttpActionResult, incluidos los resultados para devolver códigos de estado específicos, contenido con formato o respuestas negociadas por contenido.

### <a name="httprequestcontext"></a>HttpRequestContext

El nuevo **HttpRequestContext** realiza un seguimiento de cualquier Estado que esté asociado a la solicitud, pero que no esté disponible inmediatamente desde la solicitud. Por ejemplo, puede usar **HttpRequestContext** para obtener los datos de ruta, la entidad de seguridad asociada a la solicitud, el certificado de cliente, el **UrlHelper** y la raíz de la ruta de acceso virtual. Puede crear fácilmente un **HttpRequestContext** para fines de pruebas unitarias.

Dado que la entidad de seguridad de la solicitud se transmite con la solicitud en lugar de depender de **Thread. CurrentPrincipal**, la entidad de seguridad está ahora disponible a lo largo de la duración de la solicitud mientras se encuentra en la canalización de la API Web.

### <a name="cors"></a>CORS

Gracias a otra gran contribución de Brock Allen, ASP.NET ahora es totalmente compatible con el uso compartido de solicitudes de origen cruzado (CORS).

La seguridad del explorador evita que una página web realice solicitudes AJAX a otro dominio. [CORS](http://www.w3.org/TR/cors/) es un estándar del W3C que permite a un servidor relajar la Directiva del mismo origen. Con CORS, un servidor puede permitir explícitamente algunas solicitudes entre orígenes y rechazar otras.

Web API 2 ahora es compatible con CORS, incluido el control automático de las solicitudes preparatorias. Para obtener más información, vea [habilitar solicitudes entre orígenes en ASP.net web API](../../../web-api/overview/security/enabling-cross-origin-requests-in-web-api.md).

### <a name="authentication-filters"></a>Filtros de autenticación

Los filtros de autenticación son un nuevo tipo de filtro en ASP.NET Web API que se ejecutan antes que los filtros de autorización en la canalización de ASP.NET Web API y permiten especificar la lógica de autenticación por acción, por controlador o globalmente para todos los controladores. Los filtros de autenticación procesan las credenciales en la solicitud y proporcionan una entidad de seguridad correspondiente. Los filtros de autenticación también pueden agregar desafíos de autenticación en respuesta a solicitudes no autorizadas.

### <a name="filter-overrides"></a>Filtrar invalidaciones

Ahora puede invalidar los filtros que se aplican a un determinado método de acción o controlador especificando un filtro de invalidación. Los filtros de invalidación especifican un conjunto de tipos de filtro que no deben ejecutarse para un ámbito determinado (acción o controlador). Esto le permite agregar filtros globales, pero excluir algunos de acciones o controladores específicos.

### <a name="owin-integration"></a>Integración OWIN

ASP.NET Web API ahora es totalmente compatible con OWIN y se puede ejecutar en cualquier host compatible con OWIN. También se incluye un **HostAuthenticationFilter** que proporciona integración con el sistema de autenticación OWIN.

Con la integración de OWIN, puede autohospedar la API Web en su propio proceso junto con otro middleware OWIN, como Signalr. Para obtener más información, consulte [uso de OWIN para Autohospedar ASP.net web API](../../../signalr/overview/deployment/tutorial-signalr-self-host.md).

<a id="TOC13"></a>
## <a name="aspnet-signalr-20"></a>ASP.NET Signalr 2,0

En las secciones siguientes se describen las características de Signalr 2,0.

- [Basado en OWIN](#builtonowin)
- [MapHubs y MapConnection ahora son MapSignalR](#MapSignalR)
- [Compatibilidad entre dominios](#crossdomain)
- [compatibilidad con iOS y Android a través de MonoTouch y MonoDroid](#mobile)
- [Cliente .NET portátil](#portable)
- [Nuevo paquete de autohospedaje](#selfhost)
- [Compatibilidad con el servidor compatible con versiones anteriores](#backwardcompat)
- [Se ha quitado la compatibilidad de servidor para .NET 4,0](#remove40)
- [Envío de un mensaje a una lista de clientes y grupos](#messagelist)
- [Enviar un mensaje a un usuario específico](#sendtouser)
- [Mejor compatibilidad con el control de errores](#errorhandling)
- [Pruebas unitarias más sencillas de los concentradores](#unittesting)
- [Control de errores de JavaScript](#javascripterror)

Para obtener un ejemplo de cómo actualizar un proyecto existente de 1. x a Signalr 2,0, consulte [actualización de un proyecto signalr 1. x](../../../signalr/overview/releases/upgrading-signalr-1x-projects-to-20.md).

<a id="builtonowin"></a>
### <a name="built-on-owin"></a>Basado en OWIN

Signalr 2,0 se ha creado completamente en [OWIN (la interfaz web abierta para .net)](http://owin.org/). Este cambio hace que el proceso de configuración de Signalr sea mucho más coherente entre las aplicaciones de Signalr hospedadas en Web y autohospedadas, pero también requiere una serie de cambios en la API.

<a id="MapSignalR"></a>

### <a name="maphubs-and-mapconnection-are-now-mapsignalr"></a>MapHubs y MapConnection ahora son MapSignalR

Por compatibilidad con los estándares OWIN, se ha cambiado el nombre de estos métodos a `MapSignalR`. `MapSignalR` se llama sin parámetros asignará todos los concentradores (como `MapHubs` en la versión 1. x); para asignar objetos **PersistentConnection** individuales, especifique el tipo de conexión como parámetro de tipo y la extensión de dirección URL para la conexión como primer argumento.

Se llama al método `MapSignalR` en una clase de inicio Owin. Visual Studio 2013 contiene una plantilla nueva para una clase de inicio Owin; para usar esta plantilla, haga lo siguiente:

1. Haga clic con el botón derecho en el proyecto.
2. Seleccione **Agregar**, **nuevo elemento.** ..
3. Seleccione **clase de inicio Owin**. Asigne a la nueva clase el nombre **Startup.CS**.

En una **aplicación Web,** la clase de inicio Owin que contiene el método `MapSignalR` se agrega al proceso de inicio de Owin mediante una entrada en el nodo configuración de la aplicación del archivo Web. config, como se muestra a continuación.

En una **aplicación autohospedada**, la clase startup se pasa como el parámetro de tipo del método `WebApp.Start`.

**Asignación de concentradores y conexiones en Signalr 1. x (desde el archivo de aplicación global en una aplicación web):** 

[!code-csharp[Main](release-notes/samples/sample5.cs)]

**Asignación de concentradores y conexiones en Signalr 2,0 (desde un archivo de clase de inicio Owin):** 

[!code-csharp[Main](release-notes/samples/sample6.cs)]

En una **aplicación autohospedada**, la clase startup se pasa como parámetro de tipo para el método `WebApp.Start`, como se muestra a continuación.

[!code-csharp[Main](release-notes/samples/sample7.cs)]

<a id="crossdomain"></a>

### <a name="cross-domain-support"></a>Compatibilidad entre dominios

En Signalr 1. x, las solicitudes entre dominios se controlaban mediante una sola marca EnableCrossDomain. Esta marca controla las solicitudes JSONP y CORS. Para mayor flexibilidad, se ha quitado toda la compatibilidad con CORS del componente de servidor de Signalr (los clientes de JavaScript todavía usan CORS normalmente si se detecta que el explorador lo admite) y se ha puesto a disposición un nuevo middleware OWIN para admitir estos escenarios.

En Signalr 2,0, si se requiere JSONP en el cliente (para admitir solicitudes entre dominios en exploradores anteriores), deberá habilitarse explícitamente estableciendo `EnableJSONP` en el objeto `HubConfiguration` en `true`, como se muestra a continuación. JSONP está deshabilitado de forma predeterminada, ya que es menos seguro que CORS.

Para agregar el nuevo middleware de CORS en Signalr 2,0, agregue la biblioteca `Microsoft.Owin.Cors` al proyecto y llame a `UseCors` antes del middleware Signalr, tal como se muestra en la sección siguiente.

**Agregando Microsoft. Owin. CORS al proyecto**: para instalar esta biblioteca, ejecute el siguiente comando en la consola del administrador de paquetes:

[!code-powershell[Main](release-notes/samples/sample8.ps1)]

Este comando agregará la versión 2.0.0 del paquete al proyecto.

**Llamando a UseCors**

En los fragmentos de código siguientes se muestra cómo implementar conexiones entre dominios en Signalr 1. x y 2,0.

**Implementación de solicitudes entre dominios en Signalr 1. x (desde el archivo de aplicación global)**

[!code-csharp[Main](release-notes/samples/sample9.cs)]

**Implementación de solicitudes entre dominios en Signalr 2,0 (desde un C# archivo de código)**

En el código siguiente se muestra cómo habilitar CORS o JSONP en un proyecto Signalr 2,0. En este ejemplo de código se usa `Map` y `RunSignalR` en lugar de `MapSignalR`, de modo que el middleware de CORS solo se ejecute para las solicitudes de Signalr que requieran compatibilidad con CORS (en lugar de para todo el tráfico de la ruta de acceso especificada en `MapSignalR`). `Map` también se puede usar para cualquier otro middleware que deba ejecutarse para un prefijo de dirección URL específico, en lugar

[!code-csharp[Main](release-notes/samples/sample10.cs)]

<a id="mobile"></a>

### <a name="ios-and-android-support-via-monotouch-and-monodroid"></a>compatibilidad con iOS y Android a través de MonoTouch y MonoDroid

Se ha agregado compatibilidad con los clientes de iOS y Android mediante componentes MonoTouch y MonoDroid de la [biblioteca de Xamarin](https://xamarin.com/). Para obtener más información sobre cómo usarlas, vea [uso de componentes de Xamarin](https://github.com/SignalR/SignalR/wiki/Building-Mono.Mobile.sln). Estos componentes estarán disponibles en el [almacén de Xamarin](https://store.xamarin.com/) cuando la versión de signalr RTW esté disponible.

<a id="portable"></a># # # Cliente .NET portátil

Para facilitar mejor el desarrollo multiplataforma, los clientes de Silverlight, WinRT y Windows Phone se han reemplazado por un solo cliente de .NET portátil que admite las siguientes plataformas:

- NET 4,5
- Silverlight 5
- WinRT (.NET para aplicaciones de la tienda Windows)
- Windows Phone 8

<a id="selfhost"></a>

### <a name="new-self-host-package"></a>Nuevo paquete de autohospedaje

Ahora hay un paquete NuGet para que sea más fácil empezar a trabajar con el autohospedador de Signalr (aplicaciones de Signalr que se hospedan en un proceso u otra aplicación, en lugar de hospedarse en un servidor Web). Para actualizar un proyecto de host propio compilado con Signalr 1. x, quite el paquete Microsoft. AspNet. Signalr. Owin y agregue el paquete Microsoft. AspNet. Signalr. SelfHost. Para obtener más información sobre cómo empezar a usar el paquete de autohospedaje, consulte [Tutorial: Autohospedaje de signalr](../../../signalr/overview/deployment/tutorial-signalr-self-host.md).

<a id="backwardcompat"></a>

### <a name="backward-compatible-server-support"></a>Compatibilidad con el servidor compatible con versiones anteriores

En versiones anteriores de Signalr, las versiones del paquete Signalr que se usaban en el cliente y el servidor debían ser idénticas. Con el fin de admitir aplicaciones cliente gruesas que serían difíciles de actualizar, Signalr 2,0 ahora admite el uso de una versión de servidor más reciente con un cliente anterior. **Nota: Signalr 2,0 no admite servidores compilados con versiones anteriores con clientes más recientes.**

<a id="remove40"></a>

### <a name="removed-server-support-for-net-40"></a>Se ha quitado la compatibilidad de servidor para .NET 4,0

Signalr 2,0 ha quitado la compatibilidad con la interoperabilidad del servidor con .NET 4,0. .NET 4,5 debe usarse con servidores Signalr 2,0. Todavía hay un cliente .NET 4,0 para Signalr 2,0.

<a id="messagelist"></a>

### <a name="sending-a-message-to-a-list-of-clients-and-groups"></a>Envío de un mensaje a una lista de clientes y grupos

En Signalr 2,0, se puede enviar un mensaje mediante una lista de identificadores de cliente y de grupo. En los fragmentos de código siguientes se muestra cómo hacerlo.

**Envío de un mensaje a una lista de clientes y grupos mediante PersistentConnection**

[!code-csharp[Main](release-notes/samples/sample11.cs)]

**Envío de un mensaje a una lista de clientes y grupos mediante hubs**

[!code-csharp[Main](release-notes/samples/sample12.cs)]

<a id="sendtouser"></a>

### <a name="sending-a-message-to-a-specific-user"></a>Enviar un mensaje a un usuario específico

Esta característica permite a los usuarios especificar qué identificador de usuario se basa en un objeto irequest a través de una nueva interfaz IUserIdProvider:

**La interfaz IUserIdProvider**

[!code-csharp[Main](release-notes/samples/sample13.cs)]

De forma predeterminada, habrá una implementación que usa el IPrincipal.Identity.Name del usuario como nombre de usuario.

En hubs, podrá enviar mensajes a estos usuarios a través de una nueva API:

**Uso de la API clients. User**

[!code-csharp[Main](release-notes/samples/sample14.cs)]

<a id="errorhandling"></a>

### <a name="better-error-handling-support"></a>Mejor compatibilidad con el control de errores

Los usuarios ahora pueden iniciar **HubException** desde cualquier invocación del concentrador. El constructor de **HubException** puede tomar un mensaje de cadena y un objeto de datos de error adicional. Signalr serializará automáticamente la excepción y la enviará al cliente, donde se usará para rechazar o suspender la invocación del método de concentrador.

El valor de **Mostrar excepciones de concentrador detallado** no tiene ningún hecho en que **HubException** se envíe de vuelta al cliente. siempre se envía.

**Código del lado servidor que muestra el envío de un HubException al cliente**

[!code-csharp[Main](release-notes/samples/sample15.cs)]

**Código de cliente de JavaScript que muestra la respuesta a un HubException enviado desde el servidor**

[!code-html[Main](release-notes/samples/sample16.html)]

**Código de cliente .NET que muestra la respuesta a un HubException enviado desde el servidor**

[!code-csharp[Main](release-notes/samples/sample17.cs)]

<a id="unittesting"></a>

### <a name="easier-unit-testing-of-hubs"></a>Pruebas unitarias más sencillas de los concentradores

Signalr 2,0 incluye una interfaz denominada `IHubCallerConnectionContext` en los concentradores que facilita la creación de invocaciones del lado cliente ficticios. En los fragmentos de código siguientes se muestra cómo usar esta interfaz con agentes de prueba populares [xUnit.net](https://github.com/xunit/xunit) y [MOQ](https://code.google.com/p/moq/).

**Signalr de pruebas unitarias con xUnit.net**

[!code-csharp[Main](release-notes/samples/sample18.cs)]

**Signalr de pruebas unitarias con MOQ**

[!code-csharp[Main](release-notes/samples/sample19.cs)]

<a id="javascripterror"></a>

### <a name="javascript-error-handling"></a>Control de errores de JavaScript

En Signalr 2,0, todas las devoluciones de llamada de control de errores de JavaScript devuelven objetos de error de JavaScript en lugar de cadenas sin formato. Esto permite a Signalr fluir información más completa a los controladores de errores. Puede obtener la excepción interna de la propiedad `source` del error.

**Código de cliente de JavaScript que controla la excepción Start. FAIL**

[!code-javascript[Main](release-notes/samples/sample20.js)]

<a id="TOC8"></a>
## <a name="aspnet-identity"></a>ASP.NET Identity

### <a name="new-aspnet-membership-system"></a>Nuevo sistema de pertenencia de ASP.NET

ASP.NET Identity es el nuevo sistema de pertenencia a aplicaciones ASP.NET. ASP.NET Identity facilita la integración de datos de perfil específicos del usuario con los datos de la aplicación. ASP.NET Identity también le permite elegir el modelo de persistencia para perfiles de usuario en la aplicación. Puede almacenar los datos en una base de datos SQL Server u otro almacén de datos, incluidos los almacenes de datos NoSQL, como Azure Storage tablas. Para obtener más información, consulte [cuentas de usuario individuales](creating-web-projects-in-visual-studio.md#indauth) en **crear proyectos Web de ASP.net en Visual Studio 2013**.

### <a name="claims-based-authentication"></a>Autenticación basada en notificaciones

ASP.NET ahora admite la autenticación basada en notificaciones, donde la identidad del usuario se representa como un conjunto de notificaciones de un emisor de confianza. Los usuarios se pueden autenticar mediante un nombre de usuario y una contraseña que se mantengan en una base de datos de aplicación, o mediante proveedores de identidades sociales (por ejemplo: cuentas de Microsoft, Facebook, Google, Twitter) o mediante cuentas de la organización a través de Azure Active Directory o Servicios de federación de Active Directory (AD FS) (ADFS).

### <a name="integration-with-azure-active-directory-and-windows-server-active-directory"></a>Integración con Azure Active Directory y Windows Server Active Directory

Ahora puede crear proyectos de ASP.NET que usen Azure Active Directory o Windows Server Active Directory (AD) para la autenticación. Para obtener más información, vea [cuentas organizativas](creating-web-projects-in-visual-studio.md#orgauth) en **creación de proyectos Web de ASP.net en Visual Studio 2013**.

### <a name="owin-integration"></a>Integración OWIN

La autenticación ASP.NET se basa ahora en middleware OWIN que se puede usar en cualquier host basado en OWIN. Para obtener más información acerca de OWIN, consulte la siguiente sección de los [componentes Owin de Microsoft](#TOC7) .

<a id="TOC7"></a>
## <a name="microsoft-owin-components"></a>Componentes de Microsoft OWIN

[Open Web Interface for .net](http://owin.org/) (OWIN) define una abstracción entre los servidores Web .net y las aplicaciones Web. OWIN desacopla la aplicación web del servidor, lo que permite que las aplicaciones web sean independientes del host. Por ejemplo, puede hospedar una aplicación web basada en OWIN en IIS o autohospedarla en un proceso personalizado.

Los cambios introducidos en los componentes OWIN de Microsoft (también denominados proyecto Katana) incluyen nuevos componentes de servidor y host, nuevas bibliotecas auxiliares y middleware, y nuevo middleware de autenticación.

Para obtener más información sobre OWIN y Katana, consulte [what's New in Owin and Katana](../../../aspnet/overview/owin-and-katana/index.md).

**Nota: las aplicaciones [OWIN](../../../aspnet/overview/owin-and-katana/an-overview-of-project-katana.md) no se pueden ejecutar en el modo clásico de IIS; deben ejecutarse en el modo integrado.**

**Nota: las aplicaciones [OWIN](../../../aspnet/overview/owin-and-katana/an-overview-of-project-katana.md) deben ejecutarse con plena confianza.**

### <a name="new-servers-and-hosts"></a>Nuevos servidores y hosts

Con esta versión, se agregaron nuevos componentes para habilitar escenarios de autohospedaje. Estos componentes incluyen los siguientes paquetes NuGet:

- **Microsoft. Owin. host. HttpListener**. Proporciona un servidor OWIN que usa **HttpListener** para realizar escuchas de solicitudes HTTP y dirigirlas a la canalización OWIN.
- **Microsoft. Owin. Hosting** proporciona una biblioteca para desarrolladores que desean hospedar automáticamente una canalización Owin en un proceso personalizado, como una aplicación de consola o un servicio de Windows.
- **OwinHost**. Proporciona un archivo ejecutable independiente que contiene `Microsoft.Owin.Hosting` y le permite hospedar automáticamente una canalización OWIN sin tener que escribir una aplicación host personalizada.

Además, el paquete de `Microsoft.Owin.Host.SystemWeb` ahora permite que el middleware proporcione sugerencias al servidor **SystemWeb** , lo que indica que se debe llamar al middleware durante una fase de canalización ASP.net específica. Esta característica es especialmente útil para el middleware de autenticación, que se debe ejecutar temprano en la canalización ASP.NET.

### <a name="helper-libraries-and-middleware"></a>Software intermedio y bibliotecas auxiliares

Aunque puede escribir componentes OWIN solo con las definiciones de tipo y función de la especificación OWIN, el nuevo paquete de `Microsoft.Owin` proporciona un conjunto de abstracciones más fácil de usar. Este paquete combina varios paquetes anteriores (por ejemplo, `Owin.Extensions`, `Owin.Types`) en un modelo de objetos único y bien estructurado que otros componentes OWIN pueden usar fácilmente. De hecho, la mayoría de los componentes OWIN de Microsoft ahora usan este paquete.

> [!NOTE]
> Las aplicaciones [OWIN](http://www.owin.org) no se pueden ejecutar en el modo clásico de IIS; deben ejecutarse en el modo integrado.

> [!NOTE]
> Las aplicaciones [OWIN](http://www.owin.org) deben ejecutarse con plena confianza.

Esta versión también incluye el paquete Microsoft. Owin. Diagnostics, que incluye middleware para validar una aplicación OWIN en ejecución, además de middleware de página de error para ayudar a investigar los errores.

### <a name="authentication-components"></a>Componentes de autenticación

Están disponibles los siguientes componentes de autenticación.

- **Microsoft. Owin. Security. ActiveDirectory**. Habilita la autenticación mediante servicios de directorio locales o basados en la nube.
- **Microsoft. Owin. Security. cookies** permite la autenticación mediante cookies. Este paquete se llamaba anteriormente `Microsoft.Owin.Security.Forms`.
- **Microsoft. Owin. Security. Facebook** permite la autenticación con el servicio basado en OAuth de Facebook.
- **Microsoft. Owin. Security. Google** permite la autenticación con el servicio basado en OpenID de Google.
- **Microsoft. Owin. Security. JWT** permite la autenticación mediante tokens JWT.
- **Microsoft. Owin. Security. MicrosoftAccount** permite la autenticación con cuentas Microsoft.
- **Microsoft. Owin. Security. OAuth**. Proporciona un servidor de autorización de OAuth y un middleware para autenticar los tokens de portador.
- **Microsoft. Owin. Security. Twitter** permite la autenticación mediante el servicio basado en OAuth de Twitter.

Esta versión también incluye el paquete de `Microsoft.Owin.Cors`, que contiene software intermedio para el procesamiento de solicitudes HTTP entre orígenes.

> [!NOTE]
> Se ha quitado la compatibilidad con la firma JWT en la versión final de Visual Studio 2013.

<a id="ef6"></a>
## <a name="entity-framework-6"></a>Entity Framework 6

Para obtener una lista de las nuevas características y otros cambios en Entity Framework 6, consulte [Entity Framework historial de versiones](https://msdn.com/data/jj574253).

<a id="TOC14"></a>
## <a name="aspnet-razor-3"></a>ASP.NET Razor 3

ASP.NET Razor 3 incluye las siguientes características nuevas:

- Compatibilidad con la edición de pestañas. Anteriormente, el comando **dar formato al documento** , la sangría automática y el formato automático en Visual Studio no funcionaban correctamente cuando se usa la opción **mantener tabulaciones** . Este cambio corrige el formato de Visual Studio para el código Razor para el formato de tabulación.
- Compatibilidad con las reglas de reescritura de direcciones URL al generar vínculos.
- Eliminación del atributo transparente en seguridad.
  > [!NOTE]
  > Se trata de un cambio importante y hace que Razor 3 sea incompatible con MVC4 y versiones anteriores, mientras que Razor 2 es incompatible con MVC5 o ensamblados compilados con MVC5.

Los problemas de Razor 3 corregidos en Visual Studio 2013 de versiones preliminares se pueden encontrar [aquí](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&status=Resolved%7cClosed&type=All&priority=All&release=All%7cv5.0%2bPreview%7cv5.0%2bRC%7cv5.0%2bRTM&assignedTo=All&component=Web%2bPages%252fRazor&reasonClosed=Fixed&sortField=LastUpdatedDate&sortDirection=Descending&page=0).

<a id="TOC15"></a>
## <a name="aspnet-app-suspend"></a>Suspensión de la aplicación ASP.NET

La suspensión de la aplicación ASP.NET es una característica de cambio de juego en el .NET Framework 4.5.1 que cambia radicalmente la experiencia del usuario y el modelo económico para hospedar un gran número de sitios de ASP.NET en un único equipo. Para obtener más información, consulte suspensión de la [aplicación ASP.net: hospedaje Web de .net compartido con capacidad de respuesta](https://blogs.msdn.com/b/dotnet/archive/2013/10/09/asp-net-app-suspend-responsive-shared-net-web-hosting.aspx).

<a id="knownissues"></a>
## <a name="known-issues-and-breaking-changes"></a>Problemas conocidos y cambios importantes

En esta sección se describen los problemas conocidos y los cambios importantes del ASP.NET and Web Tools para Visual Studio 2013.

### <a name="nuget"></a>NuGet

- La [nueva restauración de paquetes no funciona en mono cuando se usa el archivo sln](https://nuget.codeplex.com/workitem/3596) , se corregirá en una próxima descarga de Nuget. exe y en la actualización del [paquete Nuget. CommandLine](http://www.nuget.org/packages/NuGet.CommandLine/) .
- La [nueva restauración de paquetes no funciona con proyectos de Wix](https://nuget.codeplex.com/workitem/3598) ; se corregirá en una próxima descarga de Nuget. exe y en la actualización del [paquete Nuget. CommandLine](http://www.nuget.org/packages/NuGet.CommandLine/) .
- [La restauración automática de paquetes no funciona en los proyectos de la carpeta de la solución](https://nuget.codeplex.com/workitem/3625) , sino que se corregirá en NuGet 2,8.

### <a name="aspnet-web-api"></a>ASP.NET Web API

1. `ODataQueryOptions<T>.ApplyTo(IQueryable)` no devuelve `IQueryable<T>` siempre, ya que se ha agregado compatibilidad con `$select` y `$expand`.

    En los ejemplos anteriores de `ODataQueryOptions<T>` siempre se convierta el valor devuelto de `ApplyTo` a `IQueryable<T>`. Esto funcionó antes porque las opciones de consulta que admitimos anteriormente (`$filter`, `$orderby`, `$skip``$top`) no cambian la forma de la consulta. Ahora que se admite `$select` y `$expand` el valor devuelto de `ApplyTo` no se `IQueryable<T>` siempre.

    [!code-csharp[Main](release-notes/samples/sample21.cs)]

    Si utiliza el código de ejemplo anterior, seguirá funcionando si el cliente no envía `$select` y `$expand`. Sin embargo, si desea admitir `$select` y `$expand` tiene que cambiar ese código a este.

    [!code-csharp[Main](release-notes/samples/sample22.cs)]
2. **Request. URL o RequestContext. URL es NULL durante una solicitud por lotes**

    En un escenario de procesamiento por lotes, **UrlHelper** es NULL cuando se accede desde **request. URL** o **RequestContext. URL**.

    Actualmente se realiza un seguimiento de este problema: [BatchRequestContext. URL es null para la solicitud por lotes](http://aspnetwebstack.codeplex.com/workitem/1301).

    La solución para este problema es crear una nueva instancia de **UrlHelper**, como en el ejemplo siguiente:

    **Crear una nueva instancia de UrlHelper**

    [!code-csharp[Main](release-notes/samples/sample23.cs)]

### <a name="aspnet-mvc"></a>ASP.NET MVC

1. Al usar MVC5 y OrgAuth, si tiene vistas que realizan la validación de AntiForgerToken, podría aparecer el siguiente error al publicar datos en la vista:

    **Error**:

    *Error de servidor en la aplicación '/'.*

    <em>Una demanda de tipo '<http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier>' o '<https://schemas.microsoft.com/accesscontrolservice/2010/07/claims/identityprovider>' no estaba presente en el ClaimsIdentity proporcionado. Para habilitar la compatibilidad con tokens antifalsificación con la autenticación basada en notificaciones, compruebe que el proveedor de notificaciones configurado está proporcionando ambas notificaciones en las instancias de ClaimsIdentity que genera. Si en su lugar el proveedor de notificaciones configurado usa un tipo de notificación diferente como identificador único, se puede configurar estableciendo la propiedad estática AntiForgeryConfig. UniqueClaimTypeIdentifier.</em>

    **Solución alternativa**:

    Agregue la siguiente línea en global. asax para corregirlo:

    `AntiForgeryConfig.UniqueClaimTypeIdentifier = ClaimTypes.Name;`

    Esto se corregirá en la próxima versión.
2. Después de actualizar una aplicación de MVC4 a MVC5, compile la solución e iníciela. Debería ver el siguiente error:

    Un System. Web. Webpages. Razor. Configuration. HostSection no se puede convertir en [B] System. Web. Webpages. Razor. Configuration. HostSection. El tipo A se origina en ' System. Web. Webpages. Razor, version = 2.0.0.0, Culture = neutral, PublicKeyToken = 31bf3856ad364e35 ' en el contexto ' default ' en la ubicación ' C:\windows\Microsoft.Net\assembly\GAC\_MSIL\System.Web.WebPages.Razor\v4.0\_2.0.0.0\_\_31bf3856ad364e35 \ System. Web. Webpages. Razor. dll '. El tipo B se origina en ' System. Web. Webpages. Razor, version = 3.0.0.0, Culture = neutral, PublicKeyToken = 31bf3856ad364e35 ' en el contexto ' default ' en la ubicación ' C:\Windows\Microsoft.NET\Framework\v4.0.30319\Temporary ASP.NET Files\root\6d05bbd0\e8b5908e\assembly\dl3\c9cbca63\f8910382\_6273ce01 \ System. Web. Webpages. Razor. dll '.

    Para corregir el error anterior, Abra *todos* los archivos Web. config (incluidos los de la carpeta views) en el proyecto y haga lo siguiente:

   1. Actualice todas las apariciones de la versión "4.0.0.0" de "System. Web. MVC" a "5.0.0.0".
   2. Actualice todas las apariciones de la versión "2.0.0.0" de "System. Web. helpers", &quot;System. Web. Webpages&quot; y &quot;System. Web. Webpages. Razor&quot; a "3.0.0.0"

      Por ejemplo, después de realizar los cambios anteriores, los enlaces de ensamblado deben tener un aspecto similar al siguiente:

      [!code-xml[Main](release-notes/samples/sample24.xml)]

      Para obtener información sobre cómo actualizar proyectos de MVC 4 a MVC 5, vea [Cómo actualizar un proyecto de ASP.NET MVC 4 y Web API a ASP.NET MVC 5 y Web API 2](../../../mvc/overview/releases/how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2.md).
3. Cuando se usa la validación del lado cliente con la validación discreta de jQuery, el mensaje de validación a veces es incorrecto para un elemento de entrada HTML con el tipo = ' número '. El error de validación de un valor requerido ("el campo Age es obligatorio") se muestra cuando se escribe un número no válido en lugar del mensaje correcto que requiere un número válido.

    Este problema suele encontrarse con el código con scaffolding para un modelo con una propiedad de entero en las vistas de creación y edición.

    Para solucionar este problema, cambie la aplicación auxiliar del editor de:

    `@Html.EditorFor(person => person.Age)`

    A:

    `@Html.TextBoxFor(person => person.Age)`
4. ASP.NET MVC 5 ya no admite la confianza parcial. Los proyectos que se vinculan a los binarios MVC o WebAPI deben quitar el atributo [SecurityTransparent](https://msdn.microsoft.com/library/system.security.securitytransparentattribute.aspx) y el atributo [AllowPartiallyTrustedCallers](https://msdn.microsoft.com/library/system.security.allowpartiallytrustedcallersattribute.aspx) . Al quitar estos atributos, se eliminarán los errores del compilador como los siguientes.

    `Attempt by security transparent method ‘MyComponent' to access security critical type 'System.Web.Mvc.MvcHtmlString' failed. Assembly 'PagedList.Mvc, Version=4.3.0.0, Culture=neutral, PublicKeyToken=abbb863e9397c5e1' is marked with the AllowPartiallyTrustedCallersAttribute, and uses the level 2 security transparency model. Level 2 transparency causes all methods in AllowPartiallyTrustedCallers assemblies to become security transparent by default, which may be the cause of this exception.`

    > Tenga en cuenta que, como efecto secundario de esto, no puede usar los ensamblados 4,0 y 5,0 en la misma aplicación. Debe actualizar todos ellos a 5,0.

### <a name="spa-template-with-facebook-authorization-may-cause-instability-in-ie-while-the-web-site-is-hosted-in-intranet-zone"></a>La plantilla SPA con autorización de Facebook puede provocar inestabilidad en IE mientras el sitio web se hospeda en la zona de intranet

La plantilla de SPA proporciona un inicio de sesión externo con Facebook. Cuando el proyecto creado con la plantilla se ejecuta localmente, el registro de puede hacer que IE se bloquee.

Solución:

1. Hospede el sitio web en la zona de Internet; de

2. Pruebe el escenario en un explorador que no sea Internet Explorer.

### <a name="web-forms-scaffolding"></a>Scaffolding de formularios Web Forms

El scaffolding de formularios Web Forms se ha quitado de VS2013 y estará disponible en una actualización futura de Visual Studio. Sin embargo, todavía puede usar la técnica scaffolding en un proyecto de formularios Web Forms agregando dependencias de MVC y generando scaffolding para MVC. El proyecto contendrá una combinación de formularios Web Forms y MVC.

Para agregar MVC al proyecto de formularios Web Forms, agregue un nuevo elemento con scaffolding y seleccione **dependencias de MVC 5**. Seleccione mínimo o completo en función de si necesita todos los archivos de contenido, como los scripts. Después, agregue un elemento con scaffolding para MVC, que creará vistas y un controlador en el proyecto.

### <a name="mvc-and-web-api-scaffolding---http-404-not-found-error"></a>Scaffolding de MVC y API Web: HTTP 404, error no encontrado

Si se produce un error al agregar un elemento con scaffolding a un proyecto, es posible que el proyecto se quede en un estado incoherente. Algunos de los cambios que se realizan son los scaffolding se revertirán, pero otros cambios, como los paquetes de NuGet instalados, no se revertirán. Si se revierten los cambios de configuración de enrutamiento, los usuarios recibirán un error HTTP 404 al navegar a elementos con scaffolding.

Solución:

- Para corregir este error para MVC, agregue un nuevo elemento con scaffolding y seleccione dependencias de MVC 5 (mínimo o completo). Este proceso agregará todos los cambios necesarios en el proyecto.
- Para corregir este error para la API Web:

  1. Agregue la clase WebApiConfig al proyecto.

      [!code-csharp[Main](release-notes/samples/sample25.cs)]

      [!code-vb[Main](release-notes/samples/sample26.vb)]
  2. Configure WebApiConfig. Register en el método de inicio de la aplicación\_en global. asax de la siguiente manera:

      [!code-csharp[Main](release-notes/samples/sample27.cs)]

      [!code-vb[Main](release-notes/samples/sample28.vb)]
