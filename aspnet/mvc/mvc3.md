---
uid: mvc/mvc3
title: ASP.NET MVC 3
author: rick-anderson
description: (incluye la actualización de las herramientas de abril de 2011) ASP.NET MVC 3 es un marco de trabajo para compilar aplicaciones web escalables basadas en estándares con un modelo de diseño bien establecido...
ms.author: riande
ms.date: 10/05/2010
ms.assetid: dddc8812-a0bc-49f9-aafb-caf2064c2b8c
msc.legacyurl: /mvc/mvc3
msc.type: content
ms.openlocfilehash: 421a06c89d4dbcb05d4080033813cc6558b7c698
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78499651"
---
# <a name="aspnet-mvc-3"></a>ASP.NET MVC 3

> *(incluye la actualización de las herramientas de abril de 2011)*
> 
> ASP.NET MVC 3 es un marco para compilar aplicaciones web escalables basadas en estándares con patrones de diseño bien establecidos y la potencia de ASP.NET y el .NET Framework.
> 
> Se instala en paralelo con ASP.NET MVC 2, por lo que empiece a usarlo hoy mismo.
> 
> Descargar el [instalador aquí](https://go.microsoft.com/fwlink/?LinkID=208140)

## <a name="top-features"></a>Características principales

- Sistema de scaffolding integrado extensible a través de NuGet
- Plantillas de proyecto habilitadas para HTML 5
- Vistas expresivas, incluido el nuevo motor de vistas de Razor
- Enlaces eficaces con inserción de dependencias y filtros de acción globales
- Compatibilidad enriquecida con JavaScript con JavaScript discreto, validación de jQuery y enlace JSON
- *Lea la lista completa de características [a continuación](#overview)*

## <a name="top-links"></a>Vínculos principales

Novedades de ASP.NET MVC 3

- Phil Haack: [publicación de ASP.NET MVC 3](http://haacked.com/archive/2011/01/13/aspnetmvc3-released.aspx)
- Scott Hanselman: [ASP.net MVC3, WebMatrix, NuGet, IIS Express y huerto publicadas: la versión Web de Microsoft de enero en contexto](http://www.hanselman.com/blog/ASPNETMVC3WebMatrixNuGetIISExpressAndOrchardReleasedTheMicrosoftJanuaryWebReleaseInContext.aspx)
- Scott Guthrie: [anuncio de la publicación de ASP.NET MVC 3, IIS Express, SQL CE 4, marco de granja de servidores Web, huerto, WebMatrix](https://weblogs.asp.net/scottgu/archive/2011/01/13/announcing-release-of-asp-net-mvc-3-iis-express-sql-ce-4-web-farm-framework-orchard-webmatrix.aspx)
- [Notas de la versión de ASP.NET MVC 3](../whitepapers/mvc3-release-notes.md)

Instalación y ayuda

- Instalación de ASP.NET MVC 3 con el [instalador de plataforma web (recomendado)](https://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MVC3)
- Instalación de ASP.NET MVC 3 con el [archivo ejecutable del instalador](https://go.microsoft.com/fwlink/?LinkID=208140)
- Instalación [de ASP.NET MVC 3 para Visual Studio 11 Developer Preview](https://go.microsoft.com/fwlink/?LinkID=208140)
- Lea el [tutorial Introducción a ASP.NET MVC 3](overview/older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md)
- Obtener ayuda y discutir ASP.NET MVC 3 en los [foros](https://forums.asp.net/1146.aspx)

<a id="overview"></a>
## <a name="aspnet-mvc-3-overview"></a>Información general de ASP.NET MVC 3

ASP.NET MVC 3 se basa en ASP.NET MVC 1 y 2, agregando excelentes características que simplifican el código y permiten una amplia extensibilidad. En este tema se proporciona información general sobre muchas de las nuevas características que se incluyen en esta versión, organizadas en las siguientes secciones:

- [Scaffolding extensible con integración de MvcScaffold](#BM_MvcScaffolding)
- [Plantillas de proyecto habilitadas para HTML 5](#BM_HTML5)
- [Motor de vistas de Razor](#BM_TheRazorViewEngine)
- [Compatibilidad con varios motores de vista](#BM_Support_for_Multiple_View_Engines)
- [Mejoras del controlador](#BM_Controller_Improvements)
- [JavaScript y Ajax](#BM_JavaScript_and_Ajax_Improvements)
- [Mejoras en la validación de modelos](#BM_Model_Validation_Improvements)
- [Mejoras en la inserción de dependencias](#BM_Dependency_Injection_Improvements)
- [Otras características nuevas](#BM_Other_New_Features)

<a id="BM_MvcScaffolding"></a>

## <a name="extensible-scaffolding-with-mvcscaffold-integration"></a>Scaffolding extensible con integración de MvcScaffold

El nuevo sistema de scaffolding facilita el reinicio y el uso productivo si es totalmente nuevo en el marco de trabajo y automatiza las tareas comunes de desarrollo si tiene experiencia y ya sabe lo que está haciendo.

Esto es compatible con el nuevo paquete de *scaffolding* de NuGet llamado **MvcScaffolding**. El término "scaffolding" se usa en muchas tecnologías de software para indicar "generación rápida de un esquema básico del software que se puede editar y personalizar". El paquete de scaffolding que estamos creando para ASP.NET MVC es muy útil en varios escenarios:

- **Si está aprendiendo ASP.NET MVC por primera vez**, ya que le ofrece una forma rápida de obtener código de trabajo útil, que puede editar y adaptar según sus necesidades. Le ahorra del trauma de mirar una página en blanco y no sabe por dónde empezar.
- **Si conoce ASP.NET MVC bien y ahora está explorando una nueva tecnología de complementos** , como un asignador relacional de objetos, un motor de vista, una biblioteca de pruebas, etc., porque el creador de esa tecnología puede haber creado también un paquete de scaffolding.
- **Si su trabajo implica crear repetidamente clases o archivos similares de algún tipo**, ya que puede crear scaffolding personalizados que generen accesorios de prueba, scripts de implementación o cualquier otra cosa que necesite. Todos los usuarios del equipo también pueden usar los scaffolding personalizados.

Otras características de MvcScaffolding incluyen:

- Compatibilidad con C# proyectos de y VB
- Compatibilidad con los motores de vista de Razor y ASPX
- Admite la técnica scaffolding en áreas de ASP.NET MVC y el uso de diseños de vista personalizados o patrones.
- Puede personalizar fácilmente la salida editando plantillas T4.
- Puede agregar nuevos scaffolding mediante lógica de PowerShell personalizada y plantillas T4 personalizadas. Estos (y todos los parámetros personalizados que haya proporcionado) aparecen automáticamente en la lista de finalización de pestañas de la consola.
- Puede obtener paquetes NuGet que contengan scaffolding adicionales para distintas tecnologías (por ejemplo, una prueba de concepto para LINQ to SQL ahora) y combinarlos y combinarlos

La actualización de ASP.NET MVC 3 Tools incluye una gran compatibilidad con Visual Studio para este sistema de scaffolding, como:

- El cuadro de diálogo Agregar controlador admite ahora el scaffolding automático completo de las acciones de creación, lectura, actualización y eliminación del controlador y las vistas correspondientes. De forma predeterminada, este código de acceso a datos de scaffolding utiliza EF Code First.
- El cuadro de diálogo Agregar controlador admite *scaffolding extensibles* a través de paquetes NuGet como *MvcScaffolding*. Esto permite conectar scaffolding personalizados en el cuadro de diálogo que le permitirá crear scaffolding para otras tecnologías de acceso a datos, como NHibernate o incluso JET con ODBCDirect si está tan inclinada.

Para obtener más información acerca de la técnica scaffolding en ASP.NET MVC 3, consulte los siguientes recursos:

- La serie post de Steve Sanderson, que incluye: 

    1. [Introducción: scaffolding Your ASP.NET MVC 3 Project with the MvcScaffolding Package](http://blog.stevensanderson.com/2011/01/13/scaffold-your-aspnet-mvc-3-project-with-the-mvcscaffolding-package/)
    2. [Uso estándar: casos de uso y opciones típicos](http://blog.stevensanderson.com/2011/01/13/mvcscaffolding-standard-usage/)
    3. [Relaciones uno a varios](http://blog.stevensanderson.com/2011/01/28/mvcscaffolding-one-to-many-relationships/)
    4. [Acciones de scaffolding y pruebas unitarias](http://blog.stevensanderson.com/2011/03/28/scaffolding-actions-and-unit-tests-with-mvcscaffolding/)
    5. [Invalidación de las plantillas T4](http://blog.stevensanderson.com/2011/04/06/mvcscaffolding-overriding-the-t4-templates/)
    6. [Esta entrada: crear scaffolding personalizados](http://blog.stevensanderson.com/2011/04/07/mvcscaffolding-creating-custom-scaffolders/)
- Publicación de Scott Hanselman de su sesión de PDC 2010 [creación de un blog con el "paquete sin nombre de web Love" de Microsoft](http://www.hanselman.com/blog/PDC10BuildingABlogWithMicrosoftUnnamedPackageOfWebLove.aspx)
- [Notas de la versión de MVC 3](../whitepapers/mvc3-release-notes.md)

<a id="BM_HTML5"></a>

## <a name="html-5-project-templates"></a>Plantillas de proyecto de HTML 5

El cuadro de diálogo nuevo proyecto incluye una casilla habilitar versiones HTML 5 de plantillas de proyecto. Estas plantillas aprovechan Modernizr 1,7 para proporcionar compatibilidad con HTML 5 y CSS 3 en exploradores de nivel inferior.

<a id="BM_TheRazorViewEngine"></a>

## <a name="the-razor-view-engine"></a>Motor de vistas de Razor

ASP.NET MVC 3 incluye un nuevo motor de vista denominado Razor que ofrece las siguientes ventajas:

- Sintaxis Razor es clara y concisa, lo que requiere un número mínimo de pulsaciones de teclas.
- Razor es fácil de aprender, en parte porque se basa en lenguajes existentes como C# y Visual Basic.
- Visual Studio incluye IntelliSense y la coloración de código para sintaxis Razor.
- Las vistas de Razor se pueden probar por unidad sin necesidad de ejecutar la aplicación o iniciar un servidor Web.

Entre las nuevas características de Razor se incluyen las siguientes:

- `@model` sintaxis para especificar el tipo que se pasa a la vista.
- Sintaxis de `@* *@` comment.
- La capacidad de especificar valores predeterminados (como `layoutpage`) una vez para todo un sitio.
- El método `Html.Raw` para mostrar texto sin codificarlo en HTML.
- Compatibilidad con el uso compartido de código entre varias vistas ( *\_viewstart. cshtml* o *\_archivos viewstart. vbhtml* ).

Razor también incluye nuevas aplicaciones auxiliares HTML, como las siguientes:

- `Chart`Operador Representa un gráfico, ofreciendo las mismas características que el control Chart en ASP.NET 4.
- `WebGrid`Operador Representa una cuadrícula de datos, completa con la funcionalidad de paginación y ordenación.
- `Crypto`Operador Utiliza algoritmos hash para crear contraseñas con valores Salt y hash correctamente.
- `WebImage`Operador Representa una imagen.
- `WebMail`Operador Envía un mensaje de correo electrónico.

Para obtener más información acerca de Razor, consulte los siguientes recursos:

- [Entrada de blog de Scott Guthrie Introducción a Razor](https://weblogs.asp.net/scottgu/archive/2010/07/02/introducing-razor.aspx)
- [Entrada de blog de Scott Guthrie Introducción a la palabra clave @model](https://weblogs.asp.net/scottgu/archive/2010/10/19/asp-net-mvc-3-new-model-directive-support-in-razor.aspx)
- [Entrada de blog de Scott Guthrie Introducción a los diseños de Razor](https://weblogs.asp.net/scottgu/archive/2010/10/22/asp-net-mvc-3-layouts.aspx)
- [Referencia rápida de la API de Razor](../web-pages/overview/api-reference/asp-net-web-pages-api-reference.md)
- [Notas de la versión de MVC 3](../whitepapers/mvc3-release-notes.md)

<a id="BM_Support_for_Multiple_View_Engines"></a>

## <a name="support-for-multiple-view-engines"></a>Compatibilidad con varios motores de vista

El cuadro de diálogo **Agregar vista** de ASP.NET MVC 3 le permite elegir el motor de vista con el que desea trabajar y el cuadro de diálogo **nuevo proyecto** le permite especificar el motor de vista predeterminado para un proyecto. Puede elegir el motor de vista de formularios Web Forms (ASPX), Razor o un motor de vista de código abierto como [Spark](http://sparkviewengine.com/), [NHaml](https://code.google.com/p/nhaml/)o [NDjango](http://ndjango.org/).

<a id="BM_Controller_Improvements"></a>

## <a name="controller-improvements"></a>Mejoras del controlador

### <a name="global-action-filters"></a>Filtros de acción globales

En ocasiones, querrá realizar la lógica antes de que se ejecute un método de acción o después de que se ejecute un método de acción. Para admitir esto, ASP.NET MVC 2 proporcionó filtros de acción. Los filtros de acción son atributos personalizados que proporcionan un medio declarativo para agregar el comportamiento de acciones previas y posteriores a métodos de acción de controlador específicos. Sin embargo, en algunos casos es posible que desee especificar el comportamiento anterior o posterior a la acción que se aplica a todos los métodos de acción. MVC 3 le permite especificar filtros globales agregándolos a la colección de `GlobalFilters`. Para obtener más información acerca de los filtros de acciones globales, consulte los siguientes recursos:

- [Blog de Scott Guthrie sobre MVC 3 Preview](https://weblogs.asp.net/scottgu/archive/2010/07/27/introducing-asp-net-mvc-3-preview-1.aspx)
- [Filtrado en ASP.NET MVC](https://msdn.microsoft.com/library/gg416513(VS.98).aspx)

### <a name="new-viewbag-property"></a>Nueva propiedad "ViewBag"

Los controladores de MVC 2 admiten una propiedad `ViewData` que permite pasar datos a una plantilla de vista mediante una API de diccionario enlazada en tiempo de ejecución. En MVC 3, también puede usar una sintaxis algo más sencilla con la propiedad `ViewBag` para lograr el mismo propósito. Por ejemplo, en lugar de escribir `ViewData["Message"]="text"`, puede escribir `ViewBag.Message="text"`. No es necesario definir clases fuertemente tipadas para utilizar la propiedad `ViewBag`. Dado que se trata de una propiedad dinámica, en su lugar, puede obtener o establecer propiedades y se resolverán dinámicamente en tiempo de ejecución. Internamente, `ViewBag` propiedades se almacenan como pares de nombre/valor en el Diccionario de `ViewData`. (Nota: en la mayoría de las versiones preliminares de MVC 3, la propiedad `ViewBag` se denominaba la propiedad `ViewModel`).

### <a name="new-actionresult-types"></a>Nuevos tipos "ActionResult"

Los siguientes tipos de `ActionResult` y métodos auxiliares correspondientes son nuevos o mejorados en MVC 3:

- [HttpNotFoundResult](https://msdn.microsoft.com/library/system.web.mvc.httpnotfoundresult(v=vs.98).aspx). Devuelve un código de Estado HTTP 404 al cliente.
- [RedirectResult](https://msdn.microsoft.com/library/system.web.mvc.redirectresult(v=VS.98).aspx). Devuelve un redireccionamiento temporal (código de Estado HTTP 302) o una redirección permanente (código de Estado HTTP 301), en función de un parámetro booleano. Junto con este cambio, la clase del [controlador](https://msdn.microsoft.com/library/system.web.mvc.controller(v=VS.98).aspx) tiene ahora tres métodos para realizar redirecciones permanentes: `RedirectPermanent`, `RedirectToRoutePermanent`y `RedirectToActionPermanent`. Estos métodos devuelven una instancia de `RedirectResult` con la propiedad `Permanent` establecida en `true`.
- [HttpStatusCodeResult](https://msdn.microsoft.com/library/system.web.mvc.httpstatuscoderesult(v=VS.98).aspx). Devuelve un código de Estado HTTP especificado por el usuario.

<a id="BM_JavaScript_and_Ajax_Improvements"></a>

## <a name="javascript-and-ajax-improvements"></a>Mejoras en JavaScript y Ajax

De forma predeterminada, las aplicaciones auxiliares de validación y Ajax en MVC 3 usan un enfoque de JavaScript discreto. JavaScript discreto evita insertar JavaScript en línea en HTML. Esto hace que el HTML sea más pequeño y menos saturado, y facilita el intercambio o la personalización de las bibliotecas de JavaScript. Las aplicaciones auxiliares de validación en MVC 3 también usan el complemento `jQueryValidate` de forma predeterminada. Si desea el comportamiento de MVC 2, puede deshabilitar JavaScript discreto mediante la configuración del archivo *Web. config* . Para obtener más información sobre las mejoras de JavaScript y Ajax, consulte los siguientes recursos:

- [Introducción básica a JavaScript discreto en el sitio de Wikipedia](http://en.wikipedia.org/wiki/Unobtrusive_JavaScript)
- [Publicación de JavaScript discreta de Brad Wilson](http://bradwilson.typepad.com/blog/2010/10/mvc3-unobtrusive-ajax.html)
- [Publicación de validación de JavaScript discreta de Brad Wilson](http://bradwilson.typepad.com/blog/2010/10/mvc3-unobtrusive-validation.html)
- [Creación de una aplicación de MVC 3 con JavaScript no intrusivo](overview/older-versions/creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript.md) (tutorial en el sitio de ASP.net)
- [Notas de la versión de MVC 3](../whitepapers/mvc3-release-notes.md)

### <a name="client-side-validation-enabled-by-default"></a>Validación del lado cliente habilitada de forma predeterminada

En versiones anteriores de MVC, debe llamar explícitamente al método `Html.EnableClientValidation` desde una vista para habilitar la validación del lado cliente. En MVC 3 ya no es necesario porque la validación del lado cliente está habilitada de forma predeterminada. (Puede deshabilitarlo mediante una configuración en el archivo *Web. config* ).

Para que la validación del lado cliente funcione, debe hacer referencia a las bibliotecas de validación de jQuery y jQuery adecuadas en el sitio. Puede hospedar esas bibliotecas en su propio servidor o hacer referencia a ellas desde una red de entrega de contenido (CDN) como las redes CDN de Microsoft o Google.

### <a name="remote-validator"></a>Validador remoto

ASP.NET MVC 3 admite la nueva clase [RemoteAttribute](https://msdn.microsoft.com/library/system.web.mvc.remoteattribute(v=VS.98).aspx) que le permite aprovechar la compatibilidad con el validador remoto del complemento de validación de jQuery. Esto permite a la biblioteca de validación del lado cliente llamar automáticamente a un método personalizado que se define en el servidor para realizar la lógica de validación que solo se puede realizar en el servidor.

En el ejemplo siguiente, el atributo `Remote` especifica que la validación del cliente llamará a una acción denominada `UserNameAvailable` en la clase `UsersController` para validar el campo `UserName`.

[!code-csharp[Main](mvc3/samples/sample1.cs)]

En el ejemplo siguiente se muestra el controlador correspondiente.

[!code-csharp[Main](mvc3/samples/sample2.cs)]

Para obtener más información sobre cómo usar el atributo `Remote`, vea [Cómo: implementar la validación remota en ASP.NET MVC](https://msdn.microsoft.com/library/gg508808(VS.98).aspx) en MSDN Library.

### <a name="json-binding-support"></a>Compatibilidad con enlaces JSON

ASP.NET MVC 3 incluye compatibilidad integrada con enlaces JSON que permite a los métodos de acción recibir datos codificados con JSON y enlazarlos a los parámetros de método de acción. Esta funcionalidad es útil en escenarios que implican plantillas de cliente y enlace de datos. (Las plantillas de cliente permiten dar formato y mostrar un único elemento de datos o un conjunto de elementos de datos mediante plantillas que se ejecutan en el cliente.) MVC 3 le permite conectar fácilmente plantillas de cliente con métodos de acción en el servidor que envían y reciben datos JSON. Para más información sobre la compatibilidad con enlaces JSON, consulte la sección **mejoras de JavaScript y Ajax** de la [entrada de blog MVC 3 Preview de Scott Guthrie](https://weblogs.asp.net/scottgu/archive/2010/07/27/introducing-asp-net-mvc-3-preview-1.aspx).

<a id="BM_Model_Validation_Improvements"></a>

## <a name="model-validation-improvements"></a>Mejoras en la validación de modelos

### <a name="dataannotations-metadata-attributes"></a>Atributos de metadatos de "DataAnnotations"

ASP.NET MVC 3 admite atributos de metadatos `DataAnnotations` como `DisplayAttribute`.

### <a name="validationattribute-class"></a>Clase "ValidationAttribute"

La clase `ValidationAttribute` se mejoró en el .NET Framework 4 para admitir una nueva sobrecarga de `IsValid` que proporciona más información sobre el contexto de validación actual, como el objeto que se está validando. Esto permite escenarios más completos en los que puede validar el valor actual basándose en otra propiedad del modelo. Por ejemplo, el nuevo atributo `CompareAttribute` le permite comparar los valores de dos propiedades de un modelo. En el ejemplo siguiente, la propiedad `ComparePassword` debe coincidir con el campo `Password` para que sea válido.

[!code-csharp[Main](mvc3/samples/sample3.cs)]

### <a name="validation-interfaces"></a>Interfaces de validación

La interfaz [IValidatableObject](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.ivalidatableobject.aspx) permite realizar la validación de nivel de modelo y permite proporcionar mensajes de error de validación que son específicos del estado del modelo general o entre dos propiedades dentro del modelo. MVC 3 ahora recupera los errores de la interfaz de `IValidatableObject` cuando el enlace de modelos, y marca o resalta automáticamente los campos afectados en una vista mediante las aplicaciones auxiliares de formularios HTML integradas.

La interfaz [IClientValidatable](https://msdn.microsoft.com/library/system.web.mvc.iclientvalidatable(v=VS.98).aspx) permite a ASP.NET MVC detectar en tiempo de ejecución si un validador admite la validación del cliente. Esta interfaz se ha diseñado para que se pueda integrar con una variedad de marcos de validación.

Para obtener más información sobre las interfaces de validación, consulte la sección mejoras en la **validación de modelos** de la [entrada de blog MVC 3 Preview de Scott Guthrie](https://weblogs.asp.net/scottgu/archive/2010/07/27/introducing-asp-net-mvc-3-preview-1.aspx). (Sin embargo, tenga en cuenta que la referencia a "IValidateObject" en el blog debe ser "IValidatableObject").

<a id="BM_Dependency_Injection_Improvements"></a>

## <a name="dependency-injection-improvements"></a>Mejoras en la inserción de dependencias

ASP.NET MVC 3 proporciona una mejor compatibilidad para aplicar la inserción de dependencias (DI) y para la integración con la inserción de dependencias o los contenedores de inversión de control (IOC). Se ha agregado compatibilidad con DI en las áreas siguientes:

- Controladores (registro e inyección de generadores de controladores, inyectar controladores).
- Vistas (registro e inserción de motores de vista, insertar dependencias en páginas de vistas).
- Filtros de acción (buscar e insertar filtros).
- Enlazadores de modelos (registro e inserción).
- Proveedores de validación de modelos (registro e inserción).
- Proveedores de metadatos del modelo (registro e inserción).
- Proveedores de valores (registro e inserción).

MVC 3 es compatible con la biblioteca del [localizador de servicios común](https://github.com/unitycontainer/commonservicelocator) y cualquier contenedor de di que admita la interfaz de `IServiceLocator` de esa biblioteca. También admite una nueva interfaz de `IDependencyResolver` que facilita la integración de los marcos de trabajo de DI.

Para obtener más información acerca de DI en MVC 3, vea los siguientes recursos:

- [Serie de entradas de blog de Brad Wilson sobre la ubicación del servicio](http://bradwilson.typepad.com/blog/2010/07/service-location-pt1-introduction.html)
- [Notas de la versión de MVC 3](../whitepapers/mvc3-release-notes.md)

<a id="BM_Other_New_Features"></a>

## <a name="other-new-features"></a>Otras características nuevas

### <a name="nuget-integration"></a>Integración de NuGet

ASP.NET MVC 3 instala y habilita automáticamente NuGet como parte de la configuración. NuGet es un administrador de paquetes de código abierto gratuito que facilita la búsqueda, instalación y uso de las bibliotecas y herramientas de .NET en sus proyectos. Funciona con todos los tipos de proyecto de Visual Studio (incluidos los formularios Web Forms de ASP.NET y ASP.NET MVC).

NuGet permite a los desarrolladores que mantienen proyectos de código abierto (por ejemplo, proyectos como MOQ, NHibernate, Ninject, StructureMap, NUnit, Windsor, RhinoMocks y Elmah) empaquetan sus bibliotecas y las registran en una galería en línea. A continuación, es fácil para los desarrolladores de .NET que deseen usar una de estas bibliotecas para buscar el paquete e instalarlo en los proyectos en los que trabajan.

Con la actualización de ASP.NET 3 Tools, las plantillas de proyecto incluyen paquetes de NuGet preinstalados de las bibliotecas de JavaScript, de modo que se pueden actualizar a través de NuGet. Entity Framework Code First también se instala previamente como un paquete NuGet.

Para más información sobre NuGet, consulte la [documentación correspondiente](https://docs.microsoft.com/nuget/).

### <a name="partial-page-output-caching"></a>Almacenamiento en caché de resultados de página parcial

ASP.NET MVC ha admitido el almacenamiento en caché de la salida de las respuestas de página completa desde la versión 1. MVC 3 también admite el almacenamiento en caché de resultados de página parcial, que permite almacenar en caché fácilmente regiones o fragmentos de una respuesta. Para obtener más información sobre el almacenamiento en caché, consulte la sección de **almacenamiento en caché de resultados de página parcial** de la [entrada de blog de Scott Guthrie en la versión candidata para lanzamiento de MVC 3](https://weblogs.asp.net/scottgu/archive/2010/11/09/announcing-the-asp-net-mvc-3-release-candidate.aspx) y en la sección **almacenamiento en caché de resultados de la acción secundaria** de las notas de la [versión](../whitepapers/mvc3-release-notes.md)

### <a name="granular-control-over-request-validation"></a>Control granular sobre la validación de solicitudes

ASP.NET MVC tiene validación de solicitudes integrada que ayuda a proteger automáticamente contra ataques XSS y inyección de HTML. Sin embargo, a veces desea deshabilitar explícitamente la validación de solicitudes, como si desea permitir que los usuarios publiquen contenido HTML (por ejemplo, en entradas de blog o contenido CMS). Ahora puede Agregar un atributo [AllowHtml](https://msdn.microsoft.com/library/system.web.mvc.allowhtmlattribute(v=VS.98).aspx) a modelos o ver modelos para deshabilitar la validación de solicitudes por cada propiedad durante el enlace de modelos. Para obtener más información acerca de la validación de solicitudes, consulte los siguientes recursos:

- La sección de **validación y JavaScript discreta** en [la entrada de blog de Scott Guthrie sobre la versión candidata para lanzamiento de MVC 3](https://weblogs.asp.net/scottgu/archive/2010/11/09/announcing-the-asp-net-mvc-3-release-candidate.aspx).
- [Notas de la versión de MVC 3](../whitepapers/mvc3-release-notes.md)

### <a name="extensible-new-project-dialog-box"></a>Cuadro de diálogo "nuevo proyecto" extensible

En ASP.NET MVC 3 puede agregar plantillas de proyecto, motores de vista y marcos de proyecto de prueba unitaria al cuadro de diálogo **nuevo proyecto** .

### <a name="template-scaffolding-improvements"></a>Mejoras en la scaffolding de plantilla

Las plantillas de scaffolding de ASP.NET MVC 3 realizan un mejor trabajo para identificar las propiedades de clave principal en los modelos y controlarlas de forma adecuada que en versiones anteriores de MVC. (Por ejemplo, las plantillas de scaffolding ahora aseguran que la clave principal no se puede aplicar con scaffolding como campo de formulario editable).

De forma predeterminada, los scaffoldings de creación y edición usan ahora la aplicación auxiliar de `Html.EditorFor` en lugar de la aplicación auxiliar de `Html.TextBoxFor`. Esto mejora la compatibilidad con los metadatos en el modelo en forma de atributos de anotación de datos cuando el cuadro de diálogo **Agregar vista** genera una vista.

### <a name="new-overloads-for-htmllabelfor-and-htmllabelformodel"></a>Nuevas sobrecargas para "HTML. LabelFor" y "HTML. LabelForModel"

Se han agregado nuevas sobrecargas de método para los métodos auxiliares `LabelFor` y `LabelForModel`. Las nuevas sobrecargas le permiten especificar o reemplazar el texto de la etiqueta.

### <a name="sessionless-controller-support"></a>Compatibilidad con controladores sin sesión

En ASP.NET MVC 3 puede indicar si desea que una clase de controlador use el estado de sesión y, si es así, si el estado de sesión debe ser de lectura/escritura o de solo lectura. Para obtener más información sobre la compatibilidad con los controladores sin sesión, consulte notas de la [versión de MVC 3](../whitepapers/mvc3-release-notes.md).

### <a name="new-additionalmetadataattribute-class"></a>Nueva clase "AdditionalMetadataAttribute"

Puede usar el atributo [AdditionalMetadata](https://msdn.microsoft.com/library/system.web.mvc.additionalmetadataattribute(v=VS.98).aspx) para rellenar el diccionario de `ModelMetadata.AdditionalValues` para una propiedad de modelo. Por ejemplo, si un modelo de vista tiene una propiedad que solo se debe mostrar a un administrador, puede anotar esa propiedad como se muestra en el ejemplo siguiente:

[!code-csharp[Main](mvc3/samples/sample4.cs)]

Estos metadatos se ponen a disposición de cualquier presentación o plantilla de editor cuando se representa un modelo de vista de producto. Depende de usted que interprete la información de los metadatos.

### <a name="accountcontroller-improvements"></a>Mejoras de AccountController

El AccountController en la plantilla de proyecto de Internet se ha mejorado considerablemente.

### <a name="new-intranet-project-template"></a>Nueva plantilla de proyecto de intranet

Se incluye una nueva plantilla de proyecto de intranet que habilita la autenticación de Windows y quita el AccountController.
