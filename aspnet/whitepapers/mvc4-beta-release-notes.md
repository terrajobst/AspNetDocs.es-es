---
uid: whitepapers/mvc4-beta-release-notes
title: ASP.NET MVC 4 | Microsoft Docs
author: rick-anderson
description: En este documento se describe la versión de ASP.NET MVC 4 beta para Visual Studio 2010.
ms.author: riande
ms.date: 09/09/2011
ms.assetid: 666407bb-81de-4319-89ba-0302c382a208
msc.legacyurl: /whitepapers/mvc4-beta-release-notes
msc.type: content
ms.openlocfilehash: 17800dfe091bbb7afb25f7f41e3bd885b882edb0
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78419845"
---
# <a name="aspnet-mvc-4"></a>ASP.NET MVC 4

> En este documento se describe la versión de ASP.NET MVC 4 beta para Visual Studio 2010.
> 
> > [!NOTE]
> > Esta no es la versión más reciente. Las notas de la versión de ASP.NET MVC 4 RC están disponibles [aquí](mvc4-release-notes.md).

- [Notas de instalación](#_Toc303253802)
- [Documentación](#_Toc303253803)
- [Compatibilidad](#_Toc303253804)
- [Requisitos de software](#_Toc303253805)
- [Actualización de un proyecto de ASP.NET MVC 3 a ASP.NET MVC 4](#_Toc303253806)
- [Nuevas características de ASP.NET MVC 4 beta](#_Toc303253807)

    - [ASP.NET Web API](#_Toc317096197) (Más información sobre ASP.NET Web API)
    - [Aplicación de una sola página de ASP.NET](#_Toc317096198)
    - [Mejoras en las plantillas de proyecto predeterminadas](#_Toc303253808)
    - [Plantilla de proyecto móvil](#_Toc303253809)
    - [Modos de presentación](#_Toc303253810)
    - [jQuery Mobile, el modificador de vista y el reemplazo del explorador](#_Toc303253811)
    - [Recetas para la generación de código en Visual Studio](#_Toc303253812)
    - [Compatibilidad de tareas con controladores asincrónicos](#_Toc303253813)
    - [SDK de Azure](#_Toc303253814)
    - [Problemas conocidos y cambios importantes](#_Toc303253815)

<a id="_Toc303253802"></a>
## <a name="installation-notes"></a>Notas de la instalación

ASP.NET MVC 4 beta para Visual Studio 2010 se puede instalar desde la [Página principal de ASP.NET MVC 4](../mvc/mvc4.md) mediante el instalador de plataforma Web.

Debe desinstalar todas las vistas previas instaladas anteriormente de ASP.NET MVC 4 antes de instalar ASP.NET MVC 4 beta.

Esta versión no es compatible con el .NET Framework 4,5 Developer Preview. Debe desinstalar .NET 4,5 Developer Preview antes de instalar la versión beta de ASP.NET MVC 4.

ASP.NET MVC 4 se puede instalar y se puede ejecutar en paralelo con ASP.NET MVC 3.

<a id="_Toc303253803"></a>
## <a name="documentation"></a>Documentación

La documentación de ASP.NET MVC está disponible en el sitio web de MSDN en la dirección URL siguiente:

[https://go.microsoft.com/fwlink/?LinkID=243043](https://go.microsoft.com/fwlink/?LinkID=243043)

Los tutoriales y otra información sobre ASP.NET MVC están disponibles en la página de MVC 4 del sitio web de ASP.NET ([https://www.asp.net/mvc/mvc4](../mvc/mvc4.md)).

<a id="_Toc303253804"></a>
## <a name="support"></a>Compatibilidad

Se trata de una versión preliminar y no se admite oficialmente. Si tiene alguna pregunta sobre cómo trabajar con esta versión, publíquela en el foro de ASP.NET MVC ([https://forums.asp.net/1146.aspx](https://forums.asp.net/1146.aspx)), donde los miembros de la comunidad de ASP.net suelen ser capaces de proporcionar soporte técnico informal.

<a id="_Toc303253805"></a>
## <a name="software-requirements"></a>Requisitos de software

Los componentes de ASP.NET MVC 4 para Visual Studio requieren PowerShell 2,0 y Visual Studio 2010 con Service Pack 1 o Visual Web Developer Express 2010 con Service Pack 1.

<a id="_Toc303253806"></a>
## <a name="upgrading-an-aspnet-mvc-3-project-to-aspnet-mvc-4"></a>Actualización de un proyecto de ASP.NET MVC 3 a ASP.NET MVC 4

ASP.NET MVC 4 se puede instalar en paralelo con ASP.NET MVC 3 en el mismo equipo, lo que proporciona flexibilidad a la hora de elegir cuándo actualizar una aplicación ASP.NET MVC 3 a ASP.NET MVC 4.

La manera más sencilla de actualizar consiste en crear un nuevo proyecto de ASP.NET MVC 4 y copiar todas las vistas, controladores, código y archivos de contenido del proyecto de MVC 3 existente en el nuevo proyecto y, a continuación, actualizar las referencias de ensamblado en el nuevo proyecto para que coincida con el proyecto anterior. Si ha realizado cambios en el archivo Web. config en el proyecto de MVC 3, también debe combinar esos cambios en el archivo Web. config en el proyecto de MVC 4.

Para actualizar manualmente una aplicación existente de ASP.NET MVC 3 a la versión 4, haga lo siguiente:

1. En todos los archivos Web. config del proyecto (hay uno en la raíz del proyecto, uno en la carpeta vistas y otro en la carpeta vistas para cada área del proyecto), reemplace cada instancia del texto siguiente:

    [!code-console[Main](mvc4-beta-release-notes/samples/sample1.cmd)]

    con el siguiente texto correspondiente:

    [!code-console[Main](mvc4-beta-release-notes/samples/sample2.cmd)]
2. En el archivo Web. config raíz, actualice el elemento *Webpages: version* a "2.0.0.0" y agregue una nueva clave *PreserveLoginUrl* que tenga el valor "true":

    [!code-xml[Main](mvc4-beta-release-notes/samples/sample3.xml)]
3. En Explorador de soluciones, elimine la referencia a *System. Web. Mvc* (que apunta a la dll de la versión 3). A continuación, agregue una referencia a *System. Web. Mvc* (v 4.0.0.0). En concreto, realice los cambios siguientes para actualizar las referencias de ensamblado. A continuación se indican los detalles:

    1. En Explorador de soluciones, elimine las referencias a los ensamblados siguientes: 

        - *System. Web. Mvc*(v 3.0.0.0)
        - *System. Web. Webpages*(v 1.0.0.0)
        - *System. Web. Razor*(v 1.0.0.0)
        - *System. Web. Webpages. Deployment*(v 1.0.0.0)
        - *System. Web. Webpages. Razor*(v 1.0.0.0)
    2. Agregue referencias a los siguientes ensamblados: 

        - *System. Web. Mvc*(v 4.0.0.0)
        - *System. Web. Webpages*(v 2.0.0.0)
        - *System. Web. Razor*(v 2.0.0.0)
        - *System. Web. Webpages. Deployment*(v 2.0.0.0)
        - *System. Web. Webpages. Razor*(v 2.0.0.0)
4. En Explorador de soluciones, haga clic con el botón secundario en el nombre del proyecto y seleccione descargar el proyecto. A continuación, vuelva a hacer clic con el botón derecho en el nombre y seleccione Editar *nombreDeProyecto*. csproj.
5. Busque el elemento *ProjectTypeGuids* y reemplace {E53F8FEA-EAE0-44A6-8774-FFD645390401} por {E3E379DF-F4C6-4180-9B81-6769533ABE47}.
6. Guarde los cambios, cierre el archivo de proyecto (. csproj) que estaba editando, haga clic con el botón derecho en el proyecto y, a continuación, seleccione volver a cargar el proyecto.
7. Si el proyecto hace referencia a cualquier biblioteca de terceros compilada con versiones anteriores de ASP.NET MVC, abra el archivo Web. config raíz y agregue los tres elementos *bindingRedirect* siguientes en la sección de *configuración* : 

    [!code-xml[Main](mvc4-beta-release-notes/samples/sample4.xml)]

<a id="_Toc303253807"></a>
## <a name="new-features-in-aspnet-mvc-4-beta"></a>Nuevas características de ASP.NET MVC 4 beta

En esta sección se describen las características que se han introducido en la versión beta de ASP.NET MVC 4.

<a id="_Toc317096197"></a>
### <a name="aspnet-web-api"></a>ASP.NET Web API

ASP.NET MVC 4 ahora incluye ASP.NET Web API, un nuevo marco de trabajo para crear servicios HTTP que pueden llegar a una amplia gama de clientes, incluidos exploradores y dispositivos móviles. ASP.NET Web API también es una plataforma ideal para la creación de servicios RESTful.

ASP.NET Web API incluye compatibilidad con las siguientes características:

- **Modelo de programación http moderno:** Acceder y manipular directamente las solicitudes y respuestas HTTP en las API Web con un nuevo modelo de objetos HTTP fuertemente tipado. El mismo modelo de programación y la canalización HTTP están disponibles de forma simétrica en el cliente a través del nuevo tipo HttpClient.
- **Compatibilidad total**con las rutas: las API Web ahora admiten el conjunto completo de funcionalidades de ruta que siempre formaban parte de la pila Web, incluidos los parámetros de ruta y las restricciones. Además, la asignación a acciones tiene compatibilidad total con las convenciones, por lo que ya no es necesario aplicar atributos como [HttpPost] a las clases y los métodos.
- **Negociación de contenido**: el cliente y el servidor pueden trabajar conjuntamente para determinar el formato correcto de los datos que se devuelven desde una API. Proporcionamos compatibilidad predeterminada con formatos codificados de URL de formulario, JSON y XML, y puede ampliar esta compatibilidad agregando sus propios formateadores, o incluso reemplazando la estrategia predeterminada de negociación de contenido.
- **Enlace y validación del modelo:** Los enlazadores de modelos proporcionan una manera sencilla de extraer datos de varias partes de una solicitud HTTP y convertir esas partes de mensaje en objetos .NET que pueden usar las acciones de la API Web.
- **Filtros:** Las API Web ahora admiten filtros, incluidos filtros conocidos como el atributo [Authorize]. Puede crear y conectar sus propios filtros para las acciones, la autorización y el control de excepciones.
- **Composición de consultas:** Con solo devolver IQueryable&lt;T&gt;, la API Web admitirá consultas a través de las convenciones de direcciones URL de OData.
- **Mejora de la capacidad de prueba de los detalles de http:** En lugar de establecer los detalles de HTTP en objetos de contexto estáticos, las acciones de la API Web ahora pueden trabajar con instancias de HttpRequestMessage y HttpResponseMessage. También existen versiones genéricas de estos objetos que permiten trabajar con los tipos personalizados además de los tipos HTTP.
- **Mejora de la inversión de control (IOC) a través de DependencyResolver:** Web API ahora usa el patrón de localizador de servicio implementado por el solucionador de dependencias de MVC para obtener instancias para muchas funciones diferentes.
- **Configuración basada en código:** La configuración de la API Web se realiza únicamente a través del código, lo que hace que los archivos de configuración estén limpios.
- **Self-host:** Las API Web se pueden hospedar en su propio proceso, además de en IIS, al mismo tiempo que se sigue usando todo el potencial de rutas y otras características de Web API.

Para obtener más detalles sobre ASP.NET Web API visite [https://www.asp.net/web-api](../web-api/index.md).

<a id="_Toc317096198"></a>
### <a name="aspnet-single-page-application"></a>Aplicación de una sola página de ASP.NET

ASP.NET MVC 4 incluye ahora una versión preliminar temprana de la experiencia para compilar aplicaciones de una sola página con interacciones del lado cliente significativas mediante JavaScript y API Web. Esta compatibilidad incluye:

- Un conjunto de bibliotecas de JavaScript para interacciones locales más enriquecidas con datos almacenados en caché
- Componentes adicionales de la API Web para la unidad de trabajo y la compatibilidad con DAL
- Una plantilla de proyecto de MVC con scaffolding para empezar a trabajar rápidamente

Para más información sobre la compatibilidad de aplicaciones de una sola página en ASP.NET MVC 4, visite [https://www.asp.net/single-page-application](../single-page-application/index.md).

<a id="_Toc303253808"></a>
### <a name="enhancements-to-default-project-templates"></a>Mejoras en las plantillas de proyecto predeterminadas

La plantilla que se usa para crear nuevos proyectos de ASP.NET MVC 4 se ha actualizado para crear un sitio web de aspecto más moderno:

![](mvc4-beta-release-notes/_static/image1.png)

Además de las mejoras cosméticas, hay una funcionalidad mejorada en la nueva plantilla. La plantilla emplea una técnica denominada representación adaptable para tener un buen aspecto en exploradores de escritorio y exploradores móviles sin ninguna personalización.

![](mvc4-beta-release-notes/_static/image2.png)

Para ver la representación adaptable en acción, puede usar un emulador móvil o simplemente intentar cambiar el tamaño de la ventana del explorador de escritorio para que sea menor. Cuando la ventana del explorador se quede lo suficientemente pequeña, cambiará el diseño de la página.

Otra mejora de la plantilla de proyecto predeterminada es el uso de JavaScript para proporcionar una interfaz de usuario más enriquecida. Los vínculos de inicio de sesión y registro que se usan en la plantilla son ejemplos de cómo usar el cuadro de diálogo jQuery UI para presentar una pantalla de inicio de sesión enriquecida:

![](mvc4-beta-release-notes/_static/image3.png)

<a id="_Toc303253809"></a>
### <a name="mobile-project-template"></a>Plantilla de proyecto móvil

Si va a iniciar un nuevo proyecto y desea crear un sitio específicamente para los exploradores móviles y de Tablet PC, puede utilizar la nueva plantilla de proyecto de aplicación móvil. Esto se basa en jQuery Mobile, una biblioteca de código abierto para compilar la interfaz de usuario optimizada para toque:

![](mvc4-beta-release-notes/_static/image4.png)

Esta plantilla contiene la misma estructura de aplicación que la plantilla de aplicación de Internet (y el código del controlador es prácticamente idéntico), pero tiene estilo con jQuery Mobile para tener buenos problemas y comportarse bien en dispositivos móviles basados en el toque. Para obtener más información sobre la estructura y el estilo de la interfaz de usuario móvil, vea el [sitio web de jQuery Mobile Project](http://jquerymobile.com/).

Si ya tiene un sitio orientado al escritorio al que desea agregar vistas optimizadas para dispositivos móviles, o si desea crear un único sitio que sirva vistas de estilo diferente para los exploradores de escritorio y móviles, puede usar la nueva característica modos de presentación. (Consulte la sección siguiente).

<a id="_Toc303253810"></a>
### <a name="display-modes"></a>Modos de presentación

La nueva característica modos de presentación permite a una aplicación seleccionar vistas según el explorador que realiza la solicitud. Por ejemplo, si un explorador de escritorio solicita la Página principal, la aplicación podría usar la plantilla Views\Home\Index.cshtml. Si un explorador móvil solicita la Página principal, es posible que la aplicación devuelva la plantilla Views\Home\Index.mobile.cshtml.

Los diseños y las particiones también se pueden invalidar para determinados tipos de explorador. Por ejemplo:

- Si la carpeta Views\Shared contiene las plantillas \_layout. cshtml y \_layout. Mobile. cshtml, de forma predeterminada, la aplicación usará \_layout. Mobile. cshtml durante las solicitudes de los exploradores móviles y \_layout. cshtml durante otras solicitudes.
- Si una carpeta contiene \_Part. cshtml y \_perpartal. Mobile. cshtml, el @Html.Partialde instrucciones ("\_perparted") representará \_subpartal. Mobile. cshtml durante las solicitudes de los exploradores móviles y \_parcialmente. cshtml durante otras solicitudes.

Si desea crear vistas, diseños o vistas parciales más específicos para otros dispositivos, puede registrar una nueva instancia de *DefaultDisplayMode* para especificar el nombre que se va a buscar cuando una solicitud cumple determinadas condiciones. Por ejemplo, puede Agregar el código siguiente al método de *Inicio de la aplicación\_* en el archivo global. asax para registrar la cadena "iPhone" como modo de presentación que se aplica cuando el explorador de iPhone de Apple realiza una solicitud:

[!code-csharp[Main](mvc4-beta-release-notes/samples/sample5.cs)]

Después de que se ejecute este código, cuando un explorador de iPhone de Apple realice una solicitud, la aplicación usará el diseño Views\Shared\\_Layout. iPhone. cshtml (si existe).

<a id="_Toc303253811"></a>
### <a name="jquery-mobile-the-view-switcher-and-browser-overriding"></a>jQuery Mobile, el modificador de vista y el reemplazo del explorador

jQuery Mobile es una biblioteca de código abierto para compilar una interfaz de usuario Web optimizada para Touch. Si desea usar jQuery Mobile con una aplicación de ASP.NET MVC 4, puede descargar e instalar un paquete de NuGet que le ayude a empezar. Para instalarlo desde la consola del administrador de paquetes de Visual Studio, escriba el siguiente comando:

[!code-powershell[Main](mvc4-beta-release-notes/samples/sample6.ps1)]

Esto instala jQuery Mobile y algunos archivos auxiliares, entre los que se incluyen los siguientes:

- Views/Shared/\_layout. Mobile. cshtml, que es un diseño basado en móviles de jQuery.
- Un componente de modificador de vista, que consta de la vista parcial views/Shared/\_ViewSwitcher. cshtml y el controlador ViewSwitcherController.cs.

Después de instalar el paquete, ejecute la aplicación con un explorador móvil (o equivalente, como el complemento de [conmutador de agente de usuario](http://chrispederick.com/work/user-agent-switcher/) de Firefox). Verá que las páginas tienen un aspecto bastante diferente, porque jQuery Mobile controla el diseño y el estilo. Para beneficiarse de esto, puede hacer lo siguiente:

- Cree invalidaciones de vista específicas para móviles como se describe anteriormente en [modos de presentación](#_Toc303253810) (por ejemplo, cree Views\Home\Index.Mobile.cshtml para invalidar Views\Home\Index.cshtml para exploradores móviles).
- Lea la [documentación de jQuery Mobile](http://jquerymobile.com/) para obtener más información sobre cómo agregar elementos de interfaz de usuario con optimización táctil en vistas móviles.

Una Convención para las páginas web optimizadas para móviles es agregar un vínculo cuyo texto es similar a la vista de escritorio o al modo de sitio completo que permite a los usuarios cambiar a una versión de escritorio de la página. El paquete jQuery. Mobile. MVC incluye un componente de selector de vista de ejemplo para este propósito. Se usa en la vista predeterminada Views\Shared\\_Layout. Mobile. cshtml y tiene este aspecto cuando se representa la página:

![](mvc4-beta-release-notes/_static/image5.png)

Si los visitantes hacen clic en el vínculo, se cambian a la versión de escritorio de la misma página.

Dado que el diseño del escritorio no incluirá un modificador de vista de forma predeterminada, los visitantes no podrán acceder al modo móvil. Para habilitar esto, agregue la siguiente referencia a *\_ViewSwitcher* a su diseño de escritorio, justo dentro del elemento *&gt;cuerpo del&lt;* :

[!code-cshtml[Main](mvc4-beta-release-notes/samples/sample7.cshtml)]

El modificador de vista utiliza una nueva característica denominada invalidación del explorador. Esta característica permite a la aplicación tratar las solicitudes como si provinieran de un explorador diferente (agente de usuario) que en realidad. En la tabla siguiente se enumeran los métodos que proporciona el explorador que reemplaza.

| `HttpContext.SetOverriddenBrowser(userAgentString)` | Omite el valor del agente de usuario real de la solicitud mediante el agente de usuario especificado. |
| --- | --- |
| `HttpContext.GetOverriddenUserAgent()` | Devuelve el valor de invalidación del agente de usuario de la solicitud o la cadena del agente de usuario real si no se ha especificado ninguna invalidación. |
| `HttpContext.GetOverriddenBrowser()` | Devuelve una instancia de *HttpBrowserCapabilitiesBase* que corresponde al agente de usuario establecido actualmente para la solicitud (real o invalidada). Puede usar este valor para obtener propiedades como *IsMobileDevice*. |
| `HttpContext.ClearOverriddenBrowser()` | Quita los agentes de usuario omitidos de la solicitud actual. |

El reemplazo del explorador es una característica principal de ASP.NET MVC 4 y está disponible incluso si no se instala el paquete jQuery. Mobile. MVC. Sin embargo, solo afecta a la selección de vistas, diseños y vistas parciales, no afecta a ninguna otra característica de ASP.NET que dependa del objeto *request. Browser* .

De forma predeterminada, la invalidación del agente de usuario se almacena mediante una cookie. Si desea almacenar la invalidación en otro lugar (por ejemplo, en una base de datos), puede reemplazar el proveedor predeterminado (*BrowserOverrideStores. Current*). La documentación de este proveedor estará disponible para acompañar a una versión posterior de ASP.NET MVC.

<a id="_Toc303253812"></a>
### <a name="recipes-for-code-generation-in-visual-studio"></a>Recetas para la generación de código en Visual Studio

La nueva característica de recetas permite que Visual Studio genere código específico de la solución en función de los paquetes que se pueden instalar con NuGet. El marco de recetas facilita a los desarrolladores la escritura de complementos de generación de código, que también puede utilizar para reemplazar los generadores de código integrados para agregar área, agregar controlador y agregar vista. Dado que las recetas se implementan como paquetes NuGet, se pueden proteger fácilmente en el control de código fuente y compartirse con todos los desarrolladores del proyecto automáticamente. También están disponibles en función de cada solución.

<a id="_Toc303253813"></a>
### <a name="task-support-for-asynchronous-controllers"></a>Compatibilidad de tareas con controladores asincrónicos

Ahora puede escribir métodos de acción asincrónicos como métodos únicos que devuelven un objeto de tipo *Task* o *task&lt;ActionResult&gt;* .

Por ejemplo, si usa visual C# 5 (o mediante el uso de la versión de [CTP asincrónica](https://msdn.microsoft.com/vstudio/async.aspx)), puede crear un método de acción asincrónico similar al siguiente:

[!code-csharp[Main](mvc4-beta-release-notes/samples/sample8.cs)]

En el método de acción anterior, las llamadas a *newsService. GetHeadlinesAsync* y *sportsService. GetScoresAsync* se llaman de forma asincrónica y no bloquean un subproceso del grupo de subprocesos.

Los métodos de acción asincrónicos que devuelven instancias de *tarea* también pueden admitir tiempos de espera. Para que el método de acción se pueda cancelar, agregue un parámetro de tipo *CancellationToken* a la Signatura del método de acción. En el ejemplo siguiente se muestra un método de acción asincrónica que tiene un tiempo de espera de 2500 milisegundos y que muestra una vista *TimedOut* al cliente si se agota el tiempo de espera.

[!code-csharp[Main](mvc4-beta-release-notes/samples/sample9.cs)]

<a id="_Toc303253814"></a>
### <a name="azure-sdk"></a>SDK de Azure

ASP.NET MVC 4 beta admite la versión 2011 1,5 de septiembre del SDK de Windows Azure.

<a id="_Toc303253815"></a>
## <a name="known-issues-and-breaking-changes"></a>Problemas conocidos y cambios importantes

- **Después de instalar ASP.NET MVC 4 beta, el editor CSHTML/VBHTML en el editor de Visual Studio 2010 el Service Pack 1 CSHTML/VBHTML puede pausarse durante mucho tiempo después de escribir fragmentos de código o JavaScript en los archivos CSHTML o VBHTML.** Esto solo se produce en las aplicaciones de ASP.NET MVC 4 que se acaban de crear y aún no se han compilado.

    La solución consiste en compilar el proyecto para obtener los ensamblados en la carpeta bin. Tenga en cuenta que si limpia el proyecto que quita los ensamblados de la carpeta bin, el problema del editor volverá a aparecer.

    Esto se corregirá en la próxima versión.
- **C#Las plantillas de proyecto de Visual Studio 11 beta contienen una cadena de conexión incorrecta en Global.asax.cs.** La conexión predeterminada especificada en el método de inicio de\_de la aplicación para los proyectos creados en Visual Studio 11 beta contiene una cadena de conexión de LocalDB que contiene una barra diagonal inversa sin escape (\) carácter. Esto produce un error de conexión al intentar acceder a un Entity Framework DbContext, que genera un SqlException.

    Para corregir este problema, escape el carácter de barra diagonal inversa en el método de inicio de la aplicación\_de Global.asax.cs para que sea como sigue:

    [!code-csharp[Main](mvc4-beta-release-notes/samples/sample10.cs)]
- **Las aplicaciones de ASP.NET MVC 4 que tienen como destino .NET 4,5 producirán una FileLoadException al intentar tener acceso al ensamblado System. net. http. dll cuando se ejecuten en .NET 4,0.** Las aplicaciones ASP.NET MVC 4 creadas en .NET 4,5 contienen una redirección de enlace que dará como resultado una FileLoadException que indica que "no se pudo cargar el archivo o ensamblado ' System .net. http ' o una de sus dependencias". Cuando la aplicación se ejecuta en un sistema con .NET 4,0 instalado. Para corregir este problema, quite el siguiente redireccionamiento de enlace de Web. config:

    [!code-xml[Main](mvc4-beta-release-notes/samples/sample11.xml)]

    El elemento de enlace de ensamblado del archivo Web. config modificado debe aparecer de la siguiente manera:

    [!code-xml[Main](mvc4-beta-release-notes/samples/sample12.xml)]
- <strong>La plantilla de elemento "agregar controlador" en proyectos de Visual Basic genera un espacio de nombres incorrecto cuando se invoca</strong><strong>desde dentro de un área.</strong> Al agregar un controlador a un área de un proyecto de ASP.NET MVC que usa Visual Basic, la plantilla de elemento inserta el espacio de nombres equivocado en el controlador. El resultado es un error de "archivo no encontrado" cuando se navega a cualquier acción del controlador.  
  
  El espacio de nombres generado omite todo lo que hay después del espacio de nombres raíz. Por ejemplo, el espacio de nombres generado es *RootNamespace* , pero debe ser *RootNamespace. areas. AreaName. Controllers* .
- **Cambios importantes en el motor de vistas de Razor.** Como parte de la reescritura del analizador de Razor, se han quitado los siguientes tipos de *System. Web. Mvc. Razor*: 

    - *ModelSpan*
    - *MvcVBRazorCodeGenerator*
    - *MvcCSharpRazorCodeGenerator*
    - *MvcVBRazorCodeParser*

  También se han quitado los siguientes métodos: 

    - *MvcCSharpRazorCodeParser. ParseInheritsStatement (System. Web. Razor. Parser. CodeBlockInfo)*
    - *MvcWebPageRazorHost. DecorateCodeGenerator (System. Web. Razor. Generator. RazorCodeGenerator)*
    - *MvcVBRazorCodeParser. ParseInheritsStatement (System. Web. Razor. Parser. CodeBlockInfo)*
- **Cuando WebMatrix. webdata. dll se incluye en el directorio/bin de una aplicación ASP.NET MVC 4, toma la dirección URL de la autenticación de formularios.** Al agregar el ensamblado WebMatrix. webdata. dll a la aplicación (por ejemplo, al seleccionar "ASP.NET Web Pages con sintaxis Razor" al usar el cuadro de diálogo Agregar dependencias que se pueden implementar), se invalidará la redirección de inicio de sesión de autenticación a/Account/Logon en lugar de/Account/login según lo esperado por el controlador de cuentas predeterminado de ASP.NET MVC. Para evitar este comportamiento y usar la dirección URL especificada ya en la sección de autenticación de Web. config, puede Agregar una appSetting denominada PreserveLoginUrl y establecerla en true: 

    [!code-xml[Main](mvc4-beta-release-notes/samples/sample13.xml)]
- **No se puede instalar el administrador de paquetes NuGet al intentar instalar ASP.NET MVC 4 para instalaciones en paralelo de Visual Studio 2010 y Visual Web Developer 2010.** Para ejecutar Visual Studio 2010 y Visual Web Developer 2010 en paralelo con ASP.NET MVC 4, debe instalar ASP.NET MVC 4 después de que ya se hayan instalado las dos versiones de Visual Studio.
- **Se produce un error en la desinstalación de ASP.NET MVC 4 Si ya se han desinstalado los requisitos previos.** Para desinstalar correctamente ASP.NET MVC 4You debe desinstalar ASP.NET MVC 4 antes de desinstalar Visual Studio.
- **La ejecución de un proyecto de API Web predeterminado muestra instrucciones que indican incorrectamente al usuario que agregue rutas mediante el método RegisterApis, que no existe.** Las rutas deben agregarse en el método RegisterRoutes mediante la tabla de rutas ASP.NET.
- **La instalación de ASP.NET MVC 4 beta interrumpe las aplicaciones ASP.NET MVC 3 RTM.** Las aplicaciones ASP.NET MVC 3 que se crearon con la versión RTM (no con la versión de actualización de las herramientas de ASP.NET MVC 3) requieren los siguientes cambios para funcionar en paralelo con ASP.NET MVC 4 beta. Al compilar el proyecto sin que estas actualizaciones se realicen, se producirán errores de compilación. 

    **Actualizaciones necesarias**

  1. En el archivo raíz Web. config, agregue una nueva entrada *&lt;appSettings&gt;* con las *páginas web de clave: version* y el valor *1.0.0.0*.

      [!code-xml[Main](mvc4-beta-release-notes/samples/sample14.xml)]
  2. En Explorador de soluciones, haga clic con el botón secundario en el nombre del proyecto y seleccione descargar el proyecto. A continuación, vuelva a hacer clic con el botón derecho en el nombre y seleccione Editar *nombreDeProyecto*. csproj.
  3. Busque las siguientes referencias de ensamblado: 

      [!code-xml[Main](mvc4-beta-release-notes/samples/sample15.xml)]

      Reemplácelos por lo siguiente:

      [!code-xml[Main](mvc4-beta-release-notes/samples/sample16.xml)]
  4. Guarde los cambios, cierre el archivo de proyecto (. csproj) que estaba editando y, a continuación, haga clic con el botón derecho en el proyecto y seleccione volver a cargar.
