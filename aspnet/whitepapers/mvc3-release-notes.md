---
uid: whitepapers/mvc3-release-notes
title: ASP.NET MVC 3 | Microsoft Docs
author: rick-anderson
description: ''
ms.author: riande
ms.date: 10/06/2010
ms.assetid: f44c166e-7e91-48a0-a6f8-d9285f3594e5
msc.legacyurl: /whitepapers/mvc3-release-notes
msc.type: content
ms.openlocfilehash: 504202068f5db4f8614bba02e8066ffecfd15b48
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78501043"
---
# <a name="aspnet-mvc-3"></a>ASP.NET MVC 3

- [Información general](#overview)
- [Notas de instalación](#installation-notes)
- [Requisitos de software](#software-requirements)
- [Documentación](#documentation)
- [Compatibilidad](#support)
- [Actualización de un proyecto de ASP.NET MVC 2 a ASP.NET MVC 3 Tools Update](#upgrading)
- [ASP.NET MVC 3 Tools Update (12 de abril de 2011)](#tu-changes)

    - [El cuadro de diálogo "agregar controlador" ahora puede aplicar scaffolding a los controladores con vistas y código de acceso a datos](#tu-AddControllerDialog)
    - [Mejoras en el cuadro de diálogo "nuevo proyecto de ASP.NET MVC 3"](#tu-ImprovementsNewDialogBox)
    - [Ahora, las plantillas de proyecto incluyen Modernizr 1,7](#tu-Modernizr)
    - [Las plantillas de proyecto incluyen versiones actualizadas de jQuery, jQuery UI y la validación de jQuery](#tu-UpdatedJQuery)
    - [Ahora, las plantillas de proyecto incluyen ADO.NET Entity Framework 4,1 como un paquete NuGet preinstalado](#tu-EF)
    - [Las plantillas de proyecto incluyen bibliotecas de JavaScript como paquetes NuGet preinstalados](#tu-JavaScriptLibsNuget)
    - [Problemas conocidos](#tu-KI)
- [ASP.NET MVC 3 RTM (13 de enero de 2011)](#MVC3RTM)

    - [Cambiar: se actualizó la versión de jQuery UI a 1.8.7](#RTM-1)
    - [Cambiar: cambió el valor predeterminado de ModelMetadataProvider a DataAnnotationsModelMetadataProvider](#RTM-2)
    - [Corrección: pegar parte de una expresión de Razor que contiene espacios en blanco hace que se invierta](#RTM-3)
    - [Problema corregido: al cambiar el nombre de un archivo de Razor abierto en el editor, se deshabilita la coloración de la sintaxis y IntelliSense](#RTM-4)
    - [Problemas conocidos](#RTM-KI)
    - [Cambios importantes](#RTM-BC)
- [ASP.NET MVC 3 versión candidata 2 (10 de diciembre de 2010)](#_Toc2)

    - [Plantillas de proyecto cambiadas para incluir jQuery 1.4.4, la validación de jQuery 1,7 y jQuery UI 1.8.6 y UI 1.8.6](#_Toc2_1)
    - [Se ha agregado la clase "AdditionalMetadataAttribute"](#_Toc2_2)
    - [Scaffolding de vista mejorado](#_Toc2_3)
    - [Se ha agregado el método html. Raw](#_Toc2_3)
    - [Se ha cambiado el nombre de la propiedad "Controller. ViewModel" y la propiedad "View" a "ViewBag"](#_Toc2_4)
    - [Se ha cambiado el nombre de la clase "ControllerSessionStateAttribute" a "SessionStateAttribute"](#_Toc2_5)
    - [Se ha cambiado el nombre de la propiedad "Fields" de RemoteAttribute a "AdditionalFields"](#_Toc2_6)
    - [Se ha cambiado el nombre de "SkipRequestValidationAttribute" a "AllowHtmlAttribute"](#_Toc2_7)
    - [Se ha cambiado el método "HTML. ValidationMessage" para mostrar el primer mensaje de error útil](#_Toc2_8)
    - [Se corrigió @model declaración para no agregar espacio en blanco al documento](#_Toc2_9)
    - [Se ha agregado la propiedad "FileExtensions" a los motores de vista para admitir nombres de archivo específicos del motor](#_Toc2_10)
    - [Se corrigió el ayudante "LabelFor" para emitir el valor correcto para el atributo "for"](#_Toc2_11)
    - [Se corrigió el método "RenderAction" para dar prioridad a los valores explícitos durante el enlace de modelos](#_Toc2_12)
    - [Cambios importantes](#_Toc2_BC)
    - [Problemas conocidos](#_Toc2_KI)
- [Candidato de versión comercial de ASP.NET MVC 3 (9 de noviembre de 2010)](#TOC_ASP_NET_3_RC)

    - [Nuevas características de ASP.NET MVC 3 RC](#_Toc276711785)
    - [Administrador de paquetes NuGet](#_Toc276711786)
    - [Cuadro de diálogo "nuevo proyecto" mejorado](#_Toc276711787)
    - [Controladores con sesión](#_Toc276711788)
    - [Nuevos atributos de validación](#_Toc276711789)
    - [Nuevas sobrecargas para los métodos "LabelFor" y "LabelForModel"](#_Toc276711790)
    - [Almacenamiento en caché de resultados de acciones secundarias](#_Toc276711791)
    - [Mejoras en el cuadro de diálogo "agregar vista"](#_Toc276711792)
    - [Validación de solicitudes granulares](#_Toc276711793)
    - [Cambios importantes](#_Toc276711794)
    - [Problemas conocidos](#_Toc276711795)
- [ASP. Notas de la versión beta de MVC 3 (6 de octubre de 2010)](#TOC_ASP_NET_3_Beta)

    - [Nuevas características de ASP.NET MVC 3 beta](#0.1__Toc274034215)
    - [Administrador de paquetes de NuPack](#0.1__Toc274034216)
    - [Cuadro de diálogo nuevo proyecto mejorado](#0.1__Toc274034217)
    - [Manera simplificada de especificar modelos fuertemente tipados en las vistas de Razor](#0.1__Toc274034218)
    - [Compatibilidad con los nuevos métodos auxiliares de ASP.NET Web Pages](#0.1__Toc274034219)
    - [Compatibilidad adicional con la inserción de dependencias](#0.1__Toc274034220)
    - [Nueva compatibilidad con Ajax no intrusivo basado en jQuery](#0.1__Toc274034221)
    - [Nueva compatibilidad con la validación de jQuery discreta](#0.1__Toc274034222)
    - [Nuevas marcas de toda la aplicación para la validación de clientes y JavaScript discreto](#0.1__Toc274034223)
    - [Nueva compatibilidad con el código que se ejecuta antes de que se ejecuten las vistas](#0.1__Toc274034224)
    - [Nueva compatibilidad con la sintaxis de Razor de VBHTML](#0.1__Toc274034225)
    - [Control más granular sobre ValidateInputAttribute](#0.1__Toc274034226)
    - [Las aplicaciones auxiliares convierten el carácter de subrayado en guiones para los nombres de atributos HTML especificados mediante objetos anónimos](#0.1__Toc274034227)
    - [Correcciones de errores](#0.1__Toc274034228)
    - [Cambios importantes](#0.1__Toc274034229)
    - [Problemas conocidos](#0.1__Toc274034230)
- [Ausencia](#0.1__Toc274034231)

<a id="overview"></a>
## <a name="overview"></a>Información general

En este documento se describe la versión de ASP.NET MVC 3 RTM para Visual Studio 2010. ASP.NET MVC es un marco para desarrollar aplicaciones web que usa el patrón Model-View-Controller (MVC). El instalador de ASP.NET MVC 3 incluye los siguientes componentes:

- Componentes en tiempo de ejecución de ASP.NET MVC 3
- ASP.NET MVC 3 herramientas de Visual Studio 2010
- ASP.NET Web Pages componentes en tiempo de ejecución
- ASP.NET Web Pages herramientas de Visual Studio 2010
- Administrador de paquetes de Microsoft para .NET (NuGet)
- Una actualización de Visual Studio 2010 que habilita la compatibilidad con sintaxis Razor. (Para obtener más información, vea el artículo de KnowledgeBase 2483190).

Las notas de todas las versiones preliminares de ASP.NET MVC 3 se encuentran en el sitio web de ASP.NET en la siguiente dirección URL:

https://www.asp.net/learn/whitepapers/mvc3-release-notes

<a id="installation-notes"></a>
## <a name="installation-notes"></a>Notas de la instalación

Para instalar ASP.NET MVC 3 RTM con el instalador de plataforma web (Web PI), visite la página siguiente:

[https://www.microsoft.com/web/gallery/install.aspx?appid=MVC3](https://www.microsoft.com/web/gallery/install.aspx?appid=MVC3)

Como alternativa, puede descargar el instalador de ASP.NET MVC 3 RTM para Visual Studio 2010 en la página siguiente:

https://go.microsoft.com/fwlink/?LinkID=208140

ASP.NET MVC 3 se puede instalar y se puede ejecutar en paralelo con ASP.NET MVC 2.

<a id="software-requirements"></a>
## <a name="software-requirements"></a>Requisitos de software

Los componentes de tiempo de ejecución de ASP.NET MVC 3 requieren el software siguiente:

- .NET Framework versión 4. 

    Las herramientas de ASP.NET MVC 3 Visual Studio 2010 requieren el siguiente software:
- Visual Studio 2010 o Visual Web Developer 2010 Express.

<a id="documentation"></a>
## <a name="documentation"></a>Documentación

La documentación de ASP.NET MVC está disponible en el sitio web de MSDN en la dirección URL siguiente:

[https://go.microsoft.com/fwlink/?LinkId=205717](https://go.microsoft.com/fwlink/?LinkId=205717)

Los tutoriales y otra información sobre ASP.NET MVC están disponibles en la página MVC del sitio web de ASP.NET en la siguiente dirección URL:

[https://www.asp.net/mvc/](../mvc/index.md)

<a id="support"></a>
## <a name="support"></a>Compatibilidad

Se trata de una versión con soporte técnico completo. Puede encontrar información sobre cómo obtener soporte técnico en el [sitio web de soporte técnico de Microsoft](https://support.microsoft.com/).

También se pueden plantear preguntas sobre esta versión en el foro de ASP.NET MVC, donde los miembros de la comunidad de ASP.NET normalmente pueden ofrecer soporte técnico informal:

[https://forums.asp.net/1146.aspx](https://forums.asp.net/1146.aspx)

<a id="upgrading"></a>
## <a name="upgrading-an-aspnet-mvc-2-project-to-aspnet-mvc-3-tools-update"></a>Actualización de un proyecto de ASP.NET MVC 2 a ASP.NET MVC 3 Tools Update

ASP.NET MVC 3 se puede instalar en paralelo con ASP.NET MVC 2 en el mismo equipo, lo que proporciona flexibilidad a la hora de elegir cuándo actualizar una aplicación ASP.NET MVC 2 a ASP.NET MVC 3.

Para actualizar manualmente una aplicación existente de ASP.NET MVC 2 a la versión 3, haga lo siguiente:

1. Cree un nuevo proyecto de ASP.NET MVC 3 vacío en el equipo. Este proyecto contendrá algunos archivos que son necesarios para la actualización.
2. Copie los siguientes archivos del proyecto de ASP.NET MVC 3 a la ubicación correspondiente del proyecto de ASP.NET MVC 2. Necesitará actualizar todas las referencias a la biblioteca jQuery para reflejar el nuevo nombre de archivo (jQuery-1.5.1.js): 

    - /Views/Web.config
    - /packages.config
    - /scripts/\*. js
    - \*/content/themes/.\*
3. Copie la carpeta *Packages* de la raíz de la solución de proyecto ASP.NET MVC 3 vacía en la raíz de la solución, que se encuentra en el directorio donde se encuentra el archivo. sln de la solución.
4. Si el proyecto de ASP.NET MVC 2 contiene áreas, copie el archivo/Views/Web.config en la carpeta *views* de cada área.
5. En ambos archivos Web.config del proyecto de ASP.NET MVC 2, busque y reemplace globalmente la versión de ASP.NET MVC. Busque lo siguiente: 

    [!code-console[Main](mvc3-release-notes/samples/sample1.cmd)]

    Reemplácelo por el siguiente:

    [!code-console[Main](mvc3-release-notes/samples/sample2.cmd)]
6. En Explorador de soluciones, elimine la referencia a *System. Web. Mvc* (que apunta a la dll de la versión 2) y, a continuación, agregue una referencia a *System. Web. Mvc* (v 3.0.0.0).
7. Agregue una referencia a System. Web. Webpages. dll y System. Web. helpers. dll. Estos ensamblados se encuentran en las carpetas siguientes: 

    - %ProgramFiles%\ Microsoft ASP.NET\ASP.NET MVC 3\Assemblies
    - %ProgramFiles%\ Microsoft ASP.NET\ASP.NET Web Pages\v1.0\Assemblies
8. En Explorador de soluciones, haga clic con el botón derecho en el nombre del proyecto y seleccione descargar el proyecto. A continuación, vuelva a hacer clic con el botón derecho en el nombre del proyecto y seleccione Editar *projectname*. csproj.
9. Busque el elemento *ProjectTypeGuids* y reemplace {F85E285D-A4E0-4152-9332-AB1D724D3325} por {E53F8FEA-EAE0-44A6-8774-FFD645390401}.
10. Guarde los cambios, haga clic con el botón derecho en el proyecto y, a continuación, seleccione volver a cargar el proyecto.
11. En el archivo Web. config raíz de la aplicación, agregue la siguiente configuración a la sección de *ensamblados* . 

    [!code-xml[Main](mvc3-release-notes/samples/sample3.xml)]
12. Si el proyecto hace referencia a cualquier biblioteca de terceros compilada mediante ASP.NET MVC 2, agregue el siguiente elemento *bindingRedirect* resaltado al archivo Web. config en la raíz de la aplicación en la sección de *configuración* : 

    [!code-xml[Main](mvc3-release-notes/samples/sample4.xml)]

<a id="tu-changes"></a>
## <a name="changes-in-aspnet-mvc-3-tools-update"></a>Cambios en ASP.NET MVC 3 Tools Update

En esta sección se describen los cambios realizados en la versión ASP.NET MVC 3 Tools Update con respecto a la versión ASP.NET MVC 3 RTM.

<a id="tu-AddControllerDialog"></a>
### <a name="add-controller-dialog-box-can-now-scaffold-controllers-with-views-and-data-access-code"></a>El cuadro de diálogo "Agregar controlador" ahora puede aplicar la técnica scaffolding a controladores con vistas y código de acceso de datos

La técnica scaffolding es una manera de generar rápidamente un controlador y vistas para una aplicación. Una vez generado el código, puede modificarlo para cumplir los requisitos del proyecto.

Para iniciar el cuadro de diálogo *Agregar controlador* en ASP.NET MVC 3, haga clic con el botón secundario en la carpeta *controllers* en *Explorador de soluciones*, haga clic en *Agregar*y, a continuación, haga clic en *controlador*. El cuadro de diálogo se ha mejorado y ahora ofrece opciones de scaffolding adicionales.

![](mvc3-release-notes/_static/image1.png)

Hay tres plantillas para scaffold disponibles de forma predeterminada.

#### <a name="empty-controller"></a>Vaciar controlador

Esta plantilla genera un archivo de controlador vacío. Esta plantilla es equivalente a no activar *Agregar acciones para los escenarios de creación, edición, detalles y eliminación* en versiones anteriores de ASP.NET MVC. Si la elige, no hay más opciones disponibles.

#### <a name="controller-with-empty-readwrite-actions"></a>Controlador con acciones de lectura y escritura

Esta plantilla genera un archivo de controlador que tiene todos los métodos de acción necesarios pero ningún código de implementación en los métodos. Esta plantilla es equivalente a activar *Agregar acciones para los escenarios de creación, edición, detalles y eliminación* en versiones anteriores de ASP.NET MVC. Si la elige, no hay más opciones disponibles.

#### <a name="controller-with-readwrite-actions-and-views-using-entity-framework"></a>Controlador con acciones de lectura y escritura y vistas, que usa Entity Framework

Esta plantilla le permite crear rápidamente una interfaz de usuario para entrada de datos que funcione. Genera código que administra diversos requisitos y escenarios comunes, como los siguientes:

- *Acceso a datos*. El código generado lee y escribe entidades en una base de datos. Funciona con el enfoque de Code First de Entity Framework Si elige una clase de contexto de datos existente o si permite que la plantilla genere una nueva clase *DbContext* . También funciona con el enfoque Entity Framework Database First o Model First si elige una clase *ObjectContext* existente.
- *Validación*. El código generado usa características de enlace de modelos y metadatos de ASP.NET MVC, por lo que los envíos de formularios se validan de acuerdo con las reglas declaradas en la clase de modelo. Esto incluye las reglas de validación integradas, como los atributos *obligatorios* y *StringLength* , y las reglas de validación personalizadas.
- *Relaciones uno a varios*. Si define relaciones uno a varios de clave externa entre las clases de modelo, el código generado producirá listas desplegables para seleccionar entidades relacionadas. Por ejemplo, puede definir las siguientes clases de modelo siguiendo Entity Framework convenciones de Code First: 

    [!code-csharp[Main](mvc3-release-notes/samples/sample5.cs)]

    Al aplicar scaffolding a un controlador para la clase *Product* , sus vistas permitirán a los usuarios elegir un objeto de *categoría* para cada instancia del *producto* .

    Esta plantilla habilita opciones adicionales en el cuadro de diálogo *Agregar controlador* . En *clase de modelo*, puede elegir cualquier clase de modelo de la solución, que determina el tipo de datos que los usuarios podrán crear o editar:
- Si desea usar Entity Framework Code First, puede elegir cualquier clase de modelo.
- Si usa Entity Framework Database First o Entity Framework Model First, asegúrese de elegir una clase de entidad definida en el modelo conceptual.

Para la *clase de contexto de datos*, puede elegir entre estas opciones:

- Si desea usar Code First y no tiene una clase de contexto de datos existente, elija * * nuevo contexto de datos * *. Una clase de contexto de datos se generará automáticamente.
- Si desea usar Code First y tener una clase de contexto de datos existente, elíjalo aquí. Se actualizará para conservar la clase de modelo que ha seleccionado.
- Si usa Database First o Model First, elija aquí la clase de contexto del objeto.

En el caso de las vistas, elija el motor de vista que quiera usar o elija ninguno si no desea aplicar scaffolding a ninguna vista.

Puede seleccionar opciones avanzadas para especificar más opciones para las vistas generadas. Por ejemplo, puede elegir el diseño o la página maestra que se va a usar.

<a id="tu-ImprovementsNewDialogBox"></a>
### <a name="improvements-to-the-aspnet-mvc-3-new-project-dialog-box"></a>Mejoras en el cuadro de diálogo "nuevo proyecto de ASP.NET MVC 3"

El cuadro de diálogo que se usa para crear nuevos proyectos de ASP.NET MVC 3 incluye varias mejoras, como se muestra a continuación.

![](mvc3-release-notes/_static/image2.png)

#### <a name="new-intranet-project-template"></a>Nueva plantilla "proyecto de intranet"

La lista de plantillas de proyecto incluye una nueva plantilla de aplicación de intranet. Esta plantilla contiene la configuración para compilar una aplicación Web mediante la autenticación de Windows en lugar de la autenticación de formularios. Dado que una aplicación de intranet requiere algunas opciones de configuración de IIS que no se pueden encapsular en una plantilla de proyecto, la plantilla incluye un archivo Léame con instrucciones sobre cómo hacer que la plantilla de proyecto funcione en IIS. La documentación de la nueva plantilla de aplicación de intranet está disponible en el sitio web de MSDN en la dirección URL siguiente:

[https://msdn.microsoft.com/library/gg703322(VS.98).aspx](https://msdn.microsoft.com/library/gg703322(VS.98).aspx)

#### <a name="project-templates-are-now-html5-enabled"></a>Las plantillas de proyecto ahora están habilitadas para HTML5

El cuadro de diálogo nuevo-proyecto contiene ahora una opción para agregar características específicas de HTML5 a las plantillas de proyecto. Al seleccionar la opción, se generan las vistas que contienen los nuevos elementos `<header>`, `<footer>`y `<navigation>` de HTML5.

Tenga en cuenta que las versiones anteriores de los exploradores no admiten etiquetas específicas de HTML5. Para solucionar esta limitación, las plantillas de proyecto HTML5 incluyen una referencia a la biblioteca Modernizr. (Consulte la sección siguiente).

<a id="tu-Modernizr"></a>
### <a name="project-templates-now-include-modernizr-17"></a>Ahora, las plantillas de proyecto incluyen Modernizr 1,7

Modernizr es una biblioteca de JavaScript que habilita la compatibilidad con CSS 3 y HTML5 en exploradores que todavía no admiten estas características. Esta biblioteca se incluye como un paquete NuGet preinstalado en plantillas para proyectos de ASP.NET MVC 3. Para obtener más información acerca de Modernizr, consulte [http://www.modernizr.com/](http://www.modernizr.com/).

<a id="tu-UpdatedJQuery"></a>
### <a name="project-templates-include-updated-versions-of-jquery-jquery-ui-and-jquery-validation"></a>Las plantillas de proyecto incluyen versiones actualizadas de jQuery, jQuery UI y la validación de jQuery

Las plantillas de proyecto ahora incluyen las siguientes versiones de los scripts de jQuery:

- 1\.5.1 de jQuery
- Validación de jQuery 1,8
- jQuery UI 1.8.11

Estas bibliotecas se incluyen como paquetes NuGet preinstalados.

<a id="tu-EF"></a>
### <a name="project-templates-now-include-adonet-entity-framework-41-as-a-pre-installed-nuget-package"></a>Ahora, las plantillas de proyecto incluyen ADO.NET Entity Framework 4,1 como un paquete NuGet preinstalado

ADO.NET Entity Framework 4,1 incluye la característica Code First. Code First es un nuevo patrón de desarrollo para el Entity Framework ADO.NET que proporciona una alternativa a los patrones de Database First y Model First existentes.

Code First se centra en la definición del modelo mediante el uso de clases POCO ("objetos CLR antiguos sin formato" C#) escrito en Visual Basic o. Estas clases se pueden asignar a una base de datos existente o usarse para generar un esquema de base de datos. Se puede proporcionar una configuración adicional mediante atributos de *DataAnnotations* o mediante el uso de API fluidas.

La documentación sobre el uso de Code Firstwith ASP.NET MVC está disponible en el sitio web de ASP.NET en las siguientes direcciones URL:

[https://www.asp.net/mvc/tutorials/getting-started-with-mvc3-part1-cs](../mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) [https://www.asp.net/entity-framework/tutorials/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application](../mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)

<a id="tu-JavaScriptLibsNuget"></a>
### <a name="project-templates-include-javascript-libraries-as-pre-installed-nuget-packages"></a>Las plantillas de proyecto incluyen bibliotecas de JavaScript como paquetes NuGet preinstalados

Al crear un nuevo proyecto de ASP.NET MVC 3, el proyecto incluye los archivos JavaScript mencionados anteriormente (por ejemplo, la biblioteca Modernizr) mediante su instalación mediante NuGet en lugar de agregarlos directamente a la carpeta scripts en la plantilla de proyecto. contenido. Esto le permite usar NuGet para actualizar los scripts a la versión más reciente cuando se lanzan nuevas versiones de los scripts.

Por ejemplo, dada la frecuencia de las nuevas versiones de jQuery, la versión de jQuery incluida en la plantilla de proyecto no estará actualizada. Sin embargo, dado que jQuery se incluye como un paquete NuGet instalado, se le notificará en el cuadro de diálogo NuGet cuando estén disponibles las versiones más recientes de jQuery.

Dado que jQuery incluye el número de versión en el nombre de archivo, la actualización de jQuery a la versión más reciente también requiere la actualización de la etiqueta `<script>` que hace referencia al archivo jQuery para usar el nuevo nombre de archivo. Otras bibliotecas de scripts incluidas no incluyen el número de versión en el nombre del script, por lo que se pueden actualizar más fácilmente a las versiones más recientes.

<a id="tu-KI"></a>
## <a name="known-issues"></a>Problemas conocidos

- En algunos casos, es posible que se produzca un error en la instalación con el mensaje "error de instalación con código de error (0x80070643)". Para obtener información sobre cómo solucionar este problema, vea el [artículo de Knowledge base 2531566](https://support.microsoft.com/kb/2531566).
- El scaffolding para agregar un controlador no es una de las entidades scaffolding que aprovechan la compatibilidad con la herencia de entidades en Entity Framework. Por ejemplo, dada una clase de *persona* base heredada por una clase *Student* , el scaffolding de la clase *Student* dará como resultado código generado que no se compila.
- La creación de un nuevo proyecto de ASP.NET MVC 3 dentro de una carpeta de soluciones produce un error *NullReferenceException* . La solución consiste en crear el proyecto ASP.NET MVC 3 en la raíz de la solución y, a continuación, moverla a la carpeta de la solución.
- IntelliSense para sintaxis Razor no funciona cuando se instala ReSharper. Si ha instalado ReSharper y desea aprovechar las ventajas de la compatibilidad con IntelliSense de Razor en ASP.NET MVC 3, consulte la entrada [IntelliSense de Razor y ReSharper](https://blogs.jetbrains.com/dotnet/2010/11/razor-intellisense-and-resharper/) en el blog de Hadi Hariri, en el que se explican las formas de usarlas juntas hoy en día.
- Durante la instalación, el cuadro de diálogo aceptación del CLUF muestra los términos de la licencia en una ventana menor de lo previsto.
- Al editar una vista de Razor (. cshtml o. *archivo vbhtml* ), vistas. ASP.NET MVC 3 no incluye ningún fragmento de código para las vistas de Razor. aspxselecting un fragmento de código para ASP.NET MVC mostrará fragmentos de código para
- Si instala ASP.NET MVC 3 para Visual Web Developer Express en un equipo donde no está instalado Visual Studio y, posteriormente, instala Visual Studio, debe reinstalar ASP.NET MVC 3. Visual Studio y Visual Web Developer Express comparten componentes que se actualizan mediante el instalador de ASP.NET MVC 3. El mismo problema se aplica si instala ASP.NET MVC 3 para Visual Studio en un equipo que no tiene Visual Web Developer Express y, posteriormente, instala Visual Web Developer Express.

<a id="MVC3RTM"></a>
## <a name="changes-in-aspnet-mvc-3-rtm"></a>Cambios en ASP.NET MVC 3 RTM

En esta sección se describen los cambios y las correcciones de errores realizados en la versión RTM de ASP.NET MVC 3 desde la versión RC2.

<a id="RTM-1"></a>
### <a name="change-updated-the-version-of-jquery-ui-to-187"></a>Cambiar: se actualizó la versión de jQuery UI a 1.8.7

Las plantillas de proyecto de MVC de ASP.NET para Visual Studio se actualizaron para incluir la versión más reciente de la biblioteca de la interfaz de usuario de jQuery. Las plantillas también incluyen el conjunto mínimo de archivos de recursos necesarios para la interfaz de usuario de jQuery, como los archivos de imagen y CSS asociados.

<a id="RTM-2"></a>
### <a name="change-changed-the-default-modelmetadataprovider-back-to-dataannotationsmodelmetadataprovider"></a>Cambiar: cambió el valor predeterminado de ModelMetadataProvider a DataAnnotationsModelMetadataProvider

La versión RC2 de ASP.NET MVC 3 presentó una clase *CachedDataAnnotationsMetadataProvider* que ofrecía almacenamiento en caché sobre la clase *DataAnnotationsModelMetadataProvider* existente como mejora del rendimiento. Sin embargo, algunos errores se han comunicado con esta implementación, por lo que el cambio se ha revertido y pasado al proyecto MVC Futures, que está disponible en [ASP.net webstack](https://github.com/aspnet/AspNetWebStack).

<a id="RTM-3"></a>
### <a name="fixed-pasting-part-of-a-razor-expression-that-contains-whitespace-results-in-it-being-reversed"></a>Corrección: pegar parte de una expresión de Razor que contiene espacios en blanco hace que se invierta

En las versiones preliminares de ASP.NET MVC 3, cuando se pega una parte de una expresión de Razor que contiene espacios en blanco en un archivo Razor, se invierte la expresión resultante. Por ejemplo, considere el siguiente bloque de código Razor:

[!code-cshtml[Main](mvc3-release-notes/samples/sample6.cshtml)]

Si selecciona el texto "primer parámetro" en el primer método y lo pega como argumento en el segundo método, el resultado es el siguiente:

[!code-cshtml[Main](mvc3-release-notes/samples/sample7.cshtml)]

El comportamiento correcto es que la operación de pegado debe tener como resultado lo siguiente:

[!code-cshtml[Main](mvc3-release-notes/samples/sample8.cshtml)]

Este problema se ha corregido en la versión RTM para que la expresión se conserve correctamente durante la operación de pegado.

<a id="RTM-4"></a>
### <a name="fixed-renaming-a-razor-file-that-is-opened-in-the-editor-disables-syntax-colorization-and-intellisense"></a>Problema corregido: al cambiar el nombre de un archivo de Razor abierto en el editor, se deshabilita la coloración de la sintaxis y IntelliSense

Cambiar el nombre de un archivo de Razor mediante Explorador de soluciones mientras el archivo se abre en la ventana del editor hace que el resaltado de sintaxis e IntelliSense deje de funcionar para ese archivo. Esto se ha corregido para que el resaltado e IntelliSense se mantengan después de un cambio de nombre.

<a id="RTM-KI"></a>
## <a name="known-issues"></a>Problemas conocidos

- Si cierra Visual Studio 2010 SP1 beta mientras la consola del administrador de paquetes NuGet está abierta, Visual Studio se bloqueará e intentará reiniciarse. Esto se corregirá en la versión RTM de Visual Studio 2010 SP1.
- El instalador de ASP.NET MVC 3 solo puede instalar una versión inicial del administrador de paquetes NuGet. Después de haber instalado la versión inicial, NuGet se puede instalar y actualizar con el administrador de extensiones de Visual Studio. Si ya tiene NuGet instalado, vaya a la galería de extensiones de Visual Studio para actualizar a la versión más reciente de NuGet.
- La creación de un nuevo proyecto de ASP.NET MVC 3 dentro de una carpeta de soluciones produce un error *NullReferenceException* . La solución consiste en crear el proyecto ASP.NET MVC 3 en la raíz de la solución y, a continuación, moverla a la carpeta de la solución.
- El instalador puede tardar mucho más que las versiones anteriores de ASP.NET MVC para completarse. Esto se debe a que actualiza los componentes de Visual Studio 2010.
- IntelliSense para sintaxis Razor no funciona cuando se instala ReSharper. Si ha instalado ReSharper y desea aprovechar las ventajas de la compatibilidad con IntelliSense de Razor en ASP.NET MVC 3, consulte la entrada [IntelliSense de Razor y ReSharper](https://blogs.jetbrains.com/dotnet/2010/11/razor-intellisense-and-resharper/) en el blog de Hadi Hariri, en el que se explican las formas de usarlas juntas hoy en día.
- Las vistas CCSHTML y VBHTML creadas con la versión beta de ASP.NET MVC 3 no tienen su acción de compilación establecida correctamente, con lo que se omiten estos tipos de vista cuando se publica el proyecto. El valor de acción de compilación para estos archivos se debe establecer en "Content". ASP.NET MVC 3 RTM corrige este problema para los nuevos archivos, pero no corrige la configuración de los archivos existentes para un proyecto creado con versiones preliminares.
- ![](mvc3-release-notes/_static/image3.png)
- Durante la instalación, el cuadro de diálogo aceptación del CLUF muestra los términos de la licencia en una ventana menor de lo previsto.
- Al editar una vista de Razor (archivo. cshtml), el elemento de menú ir a controlador de Visual Studio no estará disponible y no habrá fragmentos de código.
- Si instala ASP.NET MVC 3 para Visual Web Developer Express en un equipo donde no está instalado Visual Studio y, posteriormente, instala Visual Studio, debe reinstalar ASP.NET MVC 3. Visual Studio y Visual Web Developer Express comparten componentes que se actualizan mediante el instalador de ASP.NET MVC 3. El mismo problema se aplica si instala ASP.NET MVC 3 para Visual Studio en un equipo que no tiene Visual Web Developer Express y, posteriormente, instala Visual Web Developer Express.

<a id="RTM-BC"></a>
## <a name="breaking-changes"></a>Cambios importantes

- En versiones anteriores de ASP.NET MVC, los filtros de acción se crean por solicitud, excepto en algunos casos. Este comportamiento nunca era un comportamiento garantizado, pero solo un detalle de implementación y el contrato para filtros era considerar sin estado. En ASP.NET MVC 3, los filtros se almacenan en caché de forma más agresiva. Por lo tanto, los filtros de acción personalizada que almacenen incorrectamente el estado de la instancia podrían interrumpirse.
- El orden de ejecución de los filtros de excepciones ha cambiado en los filtros de excepciones que tienen el mismo valor de *orden* . En ASP.NET MVC 2 y versiones anteriores, los filtros de excepción en el controlador que tienen el mismo valor de *orden* que los de un método de acción se ejecutan antes que los filtros de excepción en el método de acción. Este suele ser el caso cuando se aplican filtros de excepción sin un valor de *orden* especificado. En ASP.NET MVC 3, este orden se ha invertido para que el controlador de excepciones más específico se ejecute primero. Como en versiones anteriores, si se especifica explícitamente la propiedad *Order* , los filtros se ejecutan en el orden especificado.
- Se ha agregado una nueva propiedad denominada *FileExtensions* a la clase base *VirtualPathProviderViewEngine* . Cuando ASP.NET busca una vista por ruta de acceso (no por nombre), solo se tienen en cuenta las vistas con una extensión de archivo incluida en la lista especificada por esta nueva propiedad. Se trata de un cambio importante en las aplicaciones donde se registra un proveedor de compilación personalizado con el fin de habilitar una extensión de archivo personalizada para las vistas de formularios Web y donde el proveedor hace referencia a esas vistas mediante una ruta de acceso completa en lugar de un nombre. La solución consiste en modificar el valor de la propiedad *FileExtensions* para incluir la extensión de archivo personalizada.
- Las implementaciones personalizadas del generador de controladores que implementan directamente la interfaz *IControllerFactory* deben proporcionar una implementación del nuevo método *GetControllerSessionBehavior* que se agregó a la interfaz en esta versión. En general, se recomienda que no implemente esta interfaz directamente y derive la clase de *DefaultControllerFactory*.

<a id="_Toc2"></a>
## <a name="changes-in-aspnet-mvc-3-rc2"></a>Cambios en ASP.NET MVC 3 RC2

En esta sección se describen los cambios (nuevas características y correcciones de errores) realizados en la versión ASP.NET MVC 3 RC2 desde la versión RC.

<a id="_Toc2_1"></a>
### <a name="project-templates-changed-to-include-jquery-144-jquery-validation-17-and-jquery-ui-186"></a>Plantillas de proyecto cambiadas para incluir jQuery 1.4.4, la validación de jQuery 1,7 y jQuery UI 1.8.6

Las plantillas de proyecto para ASP.NET MVC 3 incluyen ahora las versiones más recientes de jQuery, la validación de jQuery y jQuery UI. jQuery UI es una nueva adición a las plantillas de proyecto y proporciona widgets de interfaz de usuario útiles. Para obtener más información sobre jQuery UI, visite su página principal: [http://jqueryui.com/](http://jqueryui.com/).

<a id="_Toc2_2"></a>
### <a name="added-additionalmetadataattribute-class"></a>Se ha agregado la clase "AdditionalMetadataAttribute"

Puede usar la clase *AdditionalMetadataAttribute* para rellenar el diccionario *ModelMetadata. AdditionalValues* para una propiedad de modelo.

Por ejemplo, supongamos que un modelo de vista tiene propiedades que solo deben mostrarse a un administrador. Ese modelo se puede anotar con el nuevo atributo usando AdminOnly como la clave y true como el valor, como en el ejemplo siguiente:

[!code-csharp[Main](mvc3-release-notes/samples/sample9.cs)]

Estos metadatos se ponen a disposición de cualquier presentación o plantilla de editor cuando se representa un modelo de vista de producto. Depende de usted como desarrollador de aplicaciones interpretar la información de los metadatos.

<a id="_Toc2_3"></a>
### <a name="improved-view-scaffolding"></a>Scaffolding de vista mejorado

Las plantillas T4 que se usan para las vistas de scaffolding ahora generan llamadas a métodos auxiliares de plantilla como *EditorFor* en lugar de aplicaciones auxiliares como *TextBoxFor*. Este cambio mejora la compatibilidad con los metadatos en el modelo en forma de atributos de anotación de datos cuando el cuadro de diálogo Agregar vista genera una vista.

El scaffolding agregar vista también incluye una mejor detección y uso de la información de clave principal en el modelo, según la Convención. Por ejemplo, el cuadro de diálogo Agregar vista usa esta información para asegurarse de que el valor de clave principal no sea un campo de formulario modificable.

Las plantillas de edición y creación predeterminadas incluyen referencias a los scripts de jQuery necesarios para la validación del cliente.

<a id="_Toc2_4"></a>
### <a name="added-htmlraw-method"></a>Se ha agregado el método html. Raw

De forma predeterminada, el motor de vistas de Razor codifica en HTML todos los valores. Por ejemplo, el fragmento de código siguiente codifica el código HTML dentro de la variable greeting para que se muestre en la página como `<strong>Hello World!</strong>`.

[!code-cshtml[Main](mvc3-release-notes/samples/sample10.cshtml)]

El nuevo método *HTML. Raw* proporciona una manera sencilla de mostrar HTML sin codificar cuando se sabe que el contenido es seguro. En el ejemplo siguiente se muestra la misma cadena, pero la cadena se representa como marcado:

[!code-cshtml[Main](mvc3-release-notes/samples/sample11.cshtml)]

<a id="_Toc2_5"></a>
### <a name="renamed-controllerviewmodel-property-and-the-view-property-to-viewbag"></a>Se ha cambiado el nombre de la propiedad "Controller. ViewModel" y la propiedad "View" a "ViewBag"

Anteriormente, la propiedad *ViewModel* del *controlador* correspondía a la propiedad *View* de una vista. Ambas propiedades proporcionan una manera de tener acceso a los valores del objeto *ViewDataDictionary* mediante la sintaxis de descriptor de acceso de propiedades dinámicas. Se ha cambiado el nombre de ambas propiedades para que sean iguales a fin de evitar confusiones y ser más coherentes.

<a id="_Toc2_6"></a>
### <a name="renamed-controllersessionstateattribute-class-to-sessionstateattribute"></a>Se ha cambiado el nombre de la clase "ControllerSessionStateAttribute" a "SessionStateAttribute"

La clase *ControllerSessionStateAttribute* se presentó en la versión RC de ASP.NET MVC 3. El nombre de la propiedad ha cambiado para ser más conciso.

<a id="_Toc2_7"></a>
### <a name="renamed-remoteattribute-fields-property-to-additionalfields"></a>Se ha cambiado el nombre de la propiedad "Fields" de RemoteAttribute a "AdditionalFields"

La propiedad *Fields* de la clase *RemoteAttribute* causó cierta confusión entre los usuarios. Cambiar el nombre de esta propiedad a *AdditionalFields* clarifica su intención.

<a id="_Toc2_8"></a>
### <a name="renamed-skiprequestvalidationattribute-to-allowhtmlattribute"></a>Se ha cambiado el nombre de "SkipRequestValidationAttribute" a "AllowHtmlAttribute"

Se ha cambiado el nombre del atributo *SkipRequestValidationAttribute* a *AllowHtmlAttribute* para que represente mejor su uso previsto.

<a id="_Toc2_9"></a>
### <a name="changed-htmlvalidationmessage-method-to-display-the-first-useful-error-message"></a>Se ha cambiado el método "HTML. ValidationMessage" para mostrar el primer mensaje de error útil

Se corrigió el método *HTML. ValidationMessage* para mostrar el primer mensaje de error útil en lugar de simplemente mostrar el primer error.

Durante el enlace de modelos, el diccionario *ModelState* se puede rellenar a partir de varios orígenes con mensajes de error sobre la propiedad, incluido el propio modelo (si implementa *IValidatableObject*), de los atributos de validación aplicados a la propiedad y de las excepciones producidas mientras se tiene acceso a la propiedad.

Cuando el método *HTML. ValidationMessage* muestra un mensaje de validación, omite las entradas de estado de modelo que incluyen una excepción, ya que normalmente no están pensadas para el usuario final. En su lugar, el método busca el primer mensaje de validación que no está asociado a una excepción y muestra el mensaje. Si no se encuentra ningún mensaje de este tipo, el valor predeterminado es un mensaje de error genérico que está asociado a la primera excepción.

<a id="_Toc2_10"></a>
### <a name="fixed-model-declaration-to-not-add-whitespace-to-the-document"></a>Se corrigió @model declaración para no agregar espacio en blanco al documento

En versiones anteriores, la declaración de `@model` en la parte superior de una vista agregaba una línea en blanco a la salida HTML representada. Esto se ha corregido para que la declaración no presente espacios en blanco.

<a id="_Toc2_11"></a>
### <a name="added-fileextensions-property-to-view-engines-to-support-engine-specific-file-names"></a>Se ha agregado la propiedad "FileExtensions" a los motores de vista para admitir nombres de archivo específicos del motor

Un motor de vista puede devolver una vista mediante una ruta de acceso de vista explícita, como en el ejemplo siguiente:

[!code-csharp[Main](mvc3-release-notes/samples/sample12.cs)]

El primer motor de vista siempre intenta representar la vista. De forma predeterminada, el motor de vista de formularios Web Forms es el primer motor de vistas; Dado que el motor de formularios Web Forms no puede representar una vista de Razor, se produce un error. Los motores de vista ahora tienen una propiedad *FileExtensions* que se usa para especificar las extensiones de archivo que admiten. Esta propiedad se comprueba cuando ASP.NET determina si un motor de vista puede representar un archivo. Se trata de un cambio importante y se incluyen más detalles en la sección de [cambios importantes](#_Toc2_BC) de este documento.

<a id="_Toc2_12"></a>
### <a name="fixed-labelfor-helper-to-emit-the-correct-value-for-the-for-attribute"></a>Se corrigió el ayudante "LabelFor" para emitir el valor correcto para el atributo "for"

Se corrigió un error en el que el método *LabelFor* representa un atributo *for* que coincide con el atributo de *nombre* del elemento de *entrada* en lugar de con su identificador. Según el W3C, el atributo *for* debe coincidir con el identificador del elemento de *entrada* .

<a id="_Toc2_13"></a>
### <a name="fixed-renderaction-method-to-give-explicit-values-precedence-during-model-binding"></a>Se corrigió el método "RenderAction" para dar prioridad a los valores explícitos durante el enlace de modelos

En versiones anteriores, los valores explícitos que se pasaron al método *RenderAction* se pasaban por alto en favor de los valores de formulario actuales durante el enlace de modelos dentro de una acción secundaria. La corrección garantiza que los valores explícitos tienen prioridad durante el enlace de modelos.

<a id="_Toc2_BC"></a>
## <a name="breaking-changes"></a>Cambios importantes

- En versiones anteriores de ASP.NET MVC, se crearon filtros de acción por solicitud, excepto en algunos casos. Este comportamiento nunca era un comportamiento garantizado, pero solo un detalle de implementación y el contrato para filtros era considerar sin estado. En ASP.NET MVC 3, los filtros se almacenan en caché de forma más agresiva. Por lo tanto, los filtros de acción personalizada que almacenen incorrectamente el estado de la instancia podrían interrumpirse.
- El orden de ejecución de los filtros de excepciones ha cambiado en los filtros de excepciones que tienen el mismo valor de *orden* . En ASP.NET MVC 2 y versiones anteriores, los filtros de excepción en el controlador que tenían el mismo valor de *orden* que los de un método de acción se ejecutaban antes que los filtros de excepción en el método de acción. Este suele ser el caso cuando se aplican filtros de excepción sin un valor de *orden* especificado. En ASP.NET MVC 3, este orden se ha invertido para que el controlador de excepciones más específico se ejecute primero. Como en versiones anteriores, si se especifica explícitamente la propiedad *Order* , los filtros se ejecutan en el orden especificado.
- Se ha agregado una nueva propiedad denominada *FileExtensions* a la clase base *VirtualPathProviderViewEngine* . Cuando ASP.NET busca una vista por ruta de acceso (no por nombre), solo se tienen en cuenta las vistas con una extensión de archivo incluida en la lista especificada por esta nueva propiedad. Se trata de un cambio importante en las aplicaciones donde se registra un proveedor de compilación personalizado con el fin de habilitar una extensión de archivo personalizada para las vistas de formularios Web y donde el proveedor hace referencia a esas vistas mediante una ruta de acceso completa en lugar de un nombre. La solución consiste en modificar el valor de la propiedad *FileExtensions* para incluir la extensión de archivo personalizada.
- Las implementaciones personalizadas del generador de controladores que implementan directamente la interfaz *IControllerFactory* deben proporcionar una implementación del nuevo método *GetControllerSessionBehavior* que se agregó a la interfaz en esta versión. En general, se recomienda que no implemente esta interfaz directamente y derive la clase de *DefaultControllerFactory*.

<a id="_Toc2_KI"></a>
## <a name="known-issues"></a>Problemas conocidos

- El instalador de ASP.NET MVC 3 solo puede instalar una versión inicial del administrador de paquetes NuGet. Después de haber instalado la versión inicial, NuGet se puede instalar y actualizar con el administrador de extensiones de Visual Studio. Si ya tiene NuGet instalado, vaya a la galería de extensiones de Visual Studio para actualizar a la versión más reciente de NuGet.
- La creación de un nuevo proyecto de ASP.NET MVC 3 dentro de una carpeta de soluciones produce un error *NullReferenceException* . La solución consiste en crear el proyecto ASP.NET MVC 3 en la raíz de la solución y, a continuación, moverla a la carpeta de la solución.
- El instalador puede tardar mucho más que las versiones anteriores de ASP.NET MVC para completarse. Esto se debe a que actualiza los componentes de Visual Studio 2010.
- IntelliSense para sintaxis Razor no funciona cuando se instala ReSharper. Si ha instalado ReSharper y desea aprovechar las ventajas de la compatibilidad con IntelliSense de Razor en ASP.NET MVC 3 RC2, consulte la entrada [IntelliSense de Razor y ReSharper](https://blogs.jetbrains.com/dotnet/2010/11/razor-intellisense-and-resharper/) en el blog de Hadi Hariri, en el que se explican las formas de usarlas en la actualidad.
- Las vistas CSHTML y VBHTML creadas con la versión beta de ASP.NET MVC 3 no tienen su acción de compilación establecida correctamente, con lo que se omiten estos tipos de vista cuando se publica el proyecto. El valor de *acción de compilación* para estos archivos se debe establecer en Content ". ASP.NET MVC 3 RC2 corrige este problema para los nuevos archivos, pero no corrige la configuración de los archivos existentes para un proyecto creado con la versión beta.![](mvc3-release-notes/_static/image4.png)
- Durante la instalación, el cuadro de diálogo aceptación del CLUF muestra los términos de la licencia en una ventana menor de lo previsto.
- Al editar una vista de Razor (archivo. cshtml), el elemento de menú ir a controlador de Visual Studio no estará disponible y no habrá fragmentos de código.
- Si instala ASP.NET MVC 3 para Visual Web Developer Express en un equipo donde no está instalado Visual Studio y, posteriormente, instala Visual Studio, debe reinstalar ASP.NET MVC 3. Visual Studio y Visual Web Developer Express comparten componentes que se actualizan mediante el instalador de ASP.NET MVC 3. El mismo problema se aplica si instala ASP.NET MVC 3 para Visual Studio en un equipo que no tiene Visual Web Developer Express y, posteriormente, instala Visual Web Developer Express.
- La instalación de ASP.NET MVC 3 RC 2 no actualiza NuGet si ya lo tiene instalado. Para actualizar NuGet, vaya al administrador de extensiones de Visual Studio y debería aparecer como una actualización disponible. Puede actualizar NuGet a la versión más reciente desde allí.

<a id="TOC_ASP_NET_3_RC"></a>
## <a name="aspnet-mvc-3-release-candidate"></a>Candidato de versión comercial de ASP.NET MVC 3

ASP.NET MVC Release Candidate se publicó el 9 de noviembre de 2010.

<a id="_Toc276711785"></a>
## <a name="new-features-in-aspnet-mvc-3-rc"></a>Nuevas características de ASP.NET MVC 3 RC

En esta sección se describen las características que se han introducido en la versión ASP.NET MVC 3 RC desde la versión beta.

<a id="_Toc276711786"></a>
### <a name="nuget-package-manager"></a>Administrador de paquetes de NuGet

ASP.NET MVC 3 incluye el administrador de paquetes NuGet (anteriormente conocido como NuPack), que es una herramienta de administración de paquetes integrada para agregar bibliotecas y herramientas a proyectos de Visual Studio. Esta herramienta automatiza los pasos que los desarrolladores llevan a cabo hoy para obtener una biblioteca en el árbol de origen.

Puede trabajar con NuGet como una herramienta de línea de comandos, como una ventana de consola integrada dentro de Visual Studio 2010, en el menú contextual de Visual Studio y como un conjunto de cmdlets de PowerShell.

Para obtener más información sobre NuGet, lea la [documentación de Nuget](https://docs.microsoft.com/nuget/).

<a id="_Toc276711787"></a>
### <a name="improved-new-project-dialog-box"></a>Cuadro de diálogo "nuevo proyecto" mejorado

Al crear un nuevo proyecto, el cuadro de diálogo nuevo proyecto le permite especificar el motor de vista, así como un tipo de proyecto ASP.NET MVC.

![](mvc3-release-notes/_static/image5.png)

En esta versión se incluye compatibilidad para modificar la lista de plantillas y los motores de vista que se muestran en el cuadro de diálogo.

Las plantillas predeterminadas son las siguientes:

Vacía. Contiene un conjunto mínimo de archivos para un proyecto de ASP.NET MVC, incluida la estructura de directorios predeterminada para los proyectos de ASP.NET MVC, un archivo site. CSS que contiene los estilos predeterminados de ASP.NET MVC y un directorio de scripts que contiene los archivos de JavaScript predeterminados.

Aplicación de Internet. Contiene la funcionalidad de ejemplo que muestra cómo usar el proveedor de pertenencia con ASP.NET MVC.

La lista de plantillas de proyecto que se muestra en el cuadro de diálogo se especifica en el registro de Windows.

<a id="_Toc276711788"></a>
### <a name="sessionless-controllers"></a>Controladores con sesión

El nuevo *ControllerSessionStateAttribute* ofrece un mayor control sobre el comportamiento del estado de sesión para los controladores mediante la especificación de un valor de enumeración [System. Web. SessionState. SessionStateBehavior](https://msdn.microsoft.com/library/system.web.sessionstate.sessionstatebehavior.aspx) .

En el ejemplo siguiente se muestra cómo desactivar el estado de sesión para todas las solicitudes a un controlador.

[!code-csharp[Main](mvc3-release-notes/samples/sample13.cs)]

En el ejemplo siguiente se muestra cómo establecer el estado de sesión de solo lectura para todas las solicitudes a un controlador.

[!code-csharp[Main](mvc3-release-notes/samples/sample14.cs)]

<a id="_Toc276711789"></a>
### <a name="new-validation-attributes"></a>Nuevos atributos de validación

#### <a name="compareattribute"></a>CompareAttribute

El nuevo atributo de validación *CompareAttribute* permite comparar los valores de dos propiedades diferentes de un modelo. En el ejemplo siguiente, la propiedad *ComparePassword* debe coincidir con el campo de *contraseña* para que sea válido.

[!code-csharp[Main](mvc3-release-notes/samples/sample15.cs)]

#### <a name="remoteattribute"></a>RemoteAttribute

El nuevo atributo de validación *RemoteAttribute* aprovecha el validador remoto del complemento de validación de jQuery, que permite a la validación del lado cliente llamar a un método en el servidor que realiza la lógica de validación real.

En el ejemplo siguiente, se aplica *RemoteAttribute* a la propiedad *username* . Al editar esta propiedad en una vista de edición, la validación de cliente llamará a una acción denominada *UserNameAvailable* en la clase *UsersController* para validar este campo.

[!code-csharp[Main](mvc3-release-notes/samples/sample16.cs)]

En el ejemplo siguiente se muestra el controlador correspondiente.

[!code-csharp[Main](mvc3-release-notes/samples/sample17.cs)]

De forma predeterminada, el nombre de la propiedad a la que se aplica el atributo se envía al método de acción como un parámetro de cadena de consulta.

<a id="_Toc276711790"></a>
### <a name="new-overloads-for-labelfor-and-labelformodel-methods"></a>Nuevas sobrecargas para los métodos "LabelFor" y "LabelForModel"

Se han agregado nuevas sobrecargas para los métodos *LabelFor* y *LabelForModel* que permiten especificar el texto de la etiqueta. En el ejemplo siguiente se muestra cómo usar estas sobrecargas.

[!code-cshtml[Main](mvc3-release-notes/samples/sample18.cshtml)]

<a id="_Toc276711791"></a>
### <a name="child-action-output-caching"></a>Almacenamiento en caché de resultados de acciones secundarias

*OutputCacheAttribute* admite el almacenamiento en caché de resultados de acciones secundarias a las que se llama mediante los métodos auxiliares *HTML. RenderAction* o *HTML. Action* . En el ejemplo siguiente se muestra una vista que llama a otra acción.

[!code-cshtml[Main](mvc3-release-notes/samples/sample19.cshtml)]

La acción *GETDATE* se anota con el *OutputCacheAttribute*:

[!code-csharp[Main](mvc3-release-notes/samples/sample20.cs)]

Cuando se ejecuta este código, el resultado de la llamada a HTML. Action ("GetDate") se almacena en la memoria caché durante 100 segundos.

<a id="_Toc276711792"></a>
### <a name="add-view-dialog-box-improvements"></a>Mejoras en el cuadro de diálogo "agregar vista"

Cuando se agrega una vista fuertemente tipada, el cuadro de diálogo Agregar vista filtra ahora más tipos no aplicables que en versiones anteriores, como muchos tipos de .NET Framework principales. Además, ahora la lista está ordenada por el nombre de clase y no por el nombre de tipo completo, lo que facilita la búsqueda de tipos. Por ejemplo, el nombre de tipo se muestra ahora como en el ejemplo siguiente:

ClassName (espacio de nombres)

En versiones anteriores, se mostraba como lo siguiente:

Espacio de nombres. ClassName

<a id="_Toc276711793"></a>
### <a name="granular-request-validation"></a>Validación de solicitudes granulares

La propiedad *Exclude* de *ValidateInputAttribute* ya no existe. En su lugar, para que se omita la validación de solicitudes para las propiedades específicas de un modelo durante el enlace de modelos, use el nuevo *SkipRequestValidationAttribute*.

Por ejemplo, supongamos que se usa un método de acción para editar una entrada de blog:

[!code-csharp[Main](mvc3-release-notes/samples/sample21.cs)]

En el ejemplo siguiente se muestra el modelo de vista para una entrada de blog.

[!code-csharp[Main](mvc3-release-notes/samples/sample22.cs)]

Cuando un usuario envía algún marcado para la propiedad Description, el enlace de modelos producirá un error debido a la validación de la solicitud. Para deshabilitar la validación de solicitudes durante el enlace de modelos para la descripción de la entrada de blog, aplique *SkipRequpestValidationAttribute* a la propiedad, como se muestra en este ejemplo:.

[!code-csharp[Main](mvc3-release-notes/samples/sample23.cs)]

Como alternativa, para desactivar la validación de solicitudes para cada propiedad del modelo, aplique *ValidateInputAttribute* con un valor de *false* al método de acción:

[!code-csharp[Main](mvc3-release-notes/samples/sample24.cs)]

<a id="_Toc276711794"></a>
## <a name="breaking-changes"></a>Cambios importantes

- El orden de ejecución de los filtros de excepciones ha cambiado en los filtros de excepciones que tienen el mismo valor de *orden* . En ASP.NET MVC 2 y versiones anteriores, los filtros de excepción en el controlador que tenían el mismo *orden* que en un método de acción se ejecutaban antes que los filtros de excepción en el método de acción. Este suele ser el caso cuando se aplican filtros de excepción sin un valor de *orden* especificado. En ASP.NET MVC 3, este orden se ha invertido para que el controlador de excepciones más específico se ejecute primero. Como en versiones anteriores, si se especifica explícitamente la propiedad *Order* , los filtros se ejecutan en el orden especificado.
- Se ha agregado una nueva propiedad denominada *FileExtensions* a la clase base *VirtualPathProviderViewEngine* . Al buscar una vista por ruta de acceso (y no por nombre), solo se tienen en cuenta las vistas con una extensión de archivo incluida en la lista especificada por esta nueva propiedad. Se trata de un cambio importante para los usuarios que registran un proveedor de compilación personalizado para habilitar una extensión de archivo personalizada para las vistas de formularios Web y que hacen referencia a esas vistas mediante una ruta de acceso completa en lugar de un nombre. La solución consiste en modificar el valor de la propiedad *FileExtensions* para incluir la extensión de archivo personalizada.

<a id="_Toc276711795"></a>
## <a name="known-issues"></a>Problemas conocidos

- El instalador puede tardar mucho más que las versiones anteriores de ASP.NET MVC para completarse porque actualiza los componentes de Visual Studio 2010.
- La opción Agregar scaffolding de vista al seleccionar astrongly vista con tipo scaffolding propiedades de solo escritura. Estos deben omitirse siempre mediante scaffolding. El cuadro de diálogo Agregar vista también scaffolding propiedades de solo lectura al generar una vista "Editar" o "crear". Las propiedades de solo lectura solo deben ser scaffolding para las vistas de presentación y de lista.
- La depuración no funciona cuando se instala ASP.NET MVC 3 junto con la versión de CTP asincrónica. ASP.NET MVC 3 no se puede instalar en paralelo con la versión de CTP asincrónica. Desinstale la versión de CTP de Async para reparar la depuración. Para obtener más información, lea [esta entrada de blog](http://drew-prog.blogspot.com/2010/11/how-to-uninstall-microsoft-aspnet-mvc-3.html) sobre cómo desinstalar todos los componentes de ASP.NET MVC 3 rc.
- Razor IntelliSense no funciona cuando se instala ReSharper. Si tiene ReSharper instalado y desea aprovechar las ventajas de la compatibilidad con IntelliSense de Razor en ASP.NET MVC 3 RC, lea [esta entrada de blog](https://blogs.jetbrains.com/dotnet/2010/11/razor-intellisense-and-resharper/) de JetBrains, en la que se explican las formas de usarlas juntas hoy en día.
- Las vistas CSHTML y VBHTML creadas con beta de ASP.NET MVC 3 no tienen su acción de compilación correcta, lo que omite su publicación. La *acción de compilación* de estos archivos debe establecerse en "Content". ASP.NET MVC 3 RC corrige este problema para los nuevos archivos, pero no corrige la configuración de los archivos existentes para un proyecto creado con la versión beta.
- El instalador puede tardar mucho más que las versiones anteriores de ASP.NET MVC para completarse porque actualiza los componentes de Visual Studio 2010.
- La opción Agregar scaffolding de vista al seleccionar una vista "Editar" de la vista fuertemente tipada propiedades de solo lectura. Del mismo modo, las propiedades de solo escritura tienen scaffolding para las vistas "Mostrar".
- Durante la instalación, el cuadro de diálogo aceptación del CLUF muestra los términos de la licencia en una ventana menor de lo previsto.
- La instalación de Visual Studio Async CTP produce un conflicto con la versión de Razor que se incluye como parte de la instalación de las herramientas de ASP.NET MVC 3. Asegúrese de que no intenta instalar Visual Studio Async CTP y la versión Razor en el mismo equipo.
- Al editar una vista de Razor (archivo. cshtml), el elemento de menú ir a controlador de Visual Studio no estará disponible y no habrá fragmentos de código.

<a id="TOC_ASP_NET_3_Beta"></a>
## <a name="aspnet-mvc-3-beta"></a>ASP.NET MVC 3 beta

ASP.NET MVC 3 beta se publicó el 6 de octubre de 2010. Las siguientes notas son específicas de la versión beta y están sujetas a las actualizaciones o los cambios a los que se hace referencia en la sección de candidato de versión ASP.NET MVC 3 anterior.

## <a id="0.1__Toc274034215"></a>Nuevas características de ASP.NET MVC 3 beta

<a id="0.1__Default_validation_system"></a>En esta sección se describen las características que se han introducido en la versión beta de ASP.NET MVC 3.

### <a id="0.1__Toc274034216"></a>Administrador de paquetes NuGet

ASP.NET MVC 3 incluye el administrador de paquetes NuGet, que es una herramienta de administración de paquetes integrada para agregar bibliotecas y herramientas a proyectos de Visual Studio. En su mayor parte, automatiza los pasos que los desarrolladores llevan a cabo hoy para obtener una biblioteca en el árbol de origen.

Puede trabajar con NuGet como una herramienta de línea de comandos, como una ventana de consola integrada dentro de Visual Studio 2010, en el menú contextual de Visual Studio y como conjunto de cmdlets de PowerShell.

Para obtener más información sobre NuGet, lea la [documentación de Nuget](https://docs.microsoft.com/nuget/).

### <a id="0.1__Toc274034217"></a>Cuadro de diálogo nuevo proyecto mejorado

Al crear un nuevo proyecto, el cuadro de diálogo nuevo proyecto le permite especificar el motor de vista, así como un tipo de proyecto ASP.NET MVC.

![](mvc3-release-notes/_static/image6.png)

En esta versión no se incluye compatibilidad para modificar la lista de plantillas y los motores de vista que aparecen en el cuadro de diálogo.

Las plantillas predeterminadas son las siguientes:

Vacía. Contiene un conjunto mínimo de archivos para un proyecto de ASP.NET MVC, incluida la estructura de directorios predeterminada para los proyectos de ASP.NET MVC, un pequeño archivo site. CSS que contiene los estilos predeterminados de ASP.NET MVC y un directorio de scripts que contiene los archivos de JavaScript predeterminados.

Aplicación de Internet. Contiene la funcionalidad de ejemplo que muestra cómo usar el proveedor de pertenencia dentro de ASP.NET MVC.

### <a id="0.1__Toc274034218"></a>Manera simplificada de especificar modelos fuertemente tipados en las vistas de Razor

La manera de especificar el tipo de modelo para las vistas de Razor fuertemente tipadas se ha simplificado mediante la nueva Directiva de @model para las vistas de CSHTML y la Directiva de @ModelType para las vistas de VBHTML. En versiones anteriores de ASP.NET MVC, se especificaría un modelo fuertemente tipado para las vistas de Razor de esta manera:

[!code-cshtml[Main](mvc3-release-notes/samples/sample25.cshtml)]

En esta versión, puede usar la siguiente sintaxis:

[!code-cshtml[Main](mvc3-release-notes/samples/sample26.cshtml)]

### <a id="0.1__Toc274034219"></a>Compatibilidad con los nuevos métodos auxiliares de ASP.NET Web Pages

La nueva tecnología de ASP.NET Web Pages incluye un conjunto de métodos auxiliares que son útiles para agregar la funcionalidad de uso común a las vistas y controladores. ASP.NET MVC 3 admite el uso de estos métodos auxiliares en los controladores y las vistas (si es necesario). Estos métodos están incluidos en el ensamblado System. Web. helpers. En la tabla siguiente se enumeran algunos de los métodos auxiliares de ASP.NET Web Pages.

| **Auxiliar** | **Descripción** |
| --- | --- |
| Chart | Representa un gráfico dentro de una vista. Contiene métodos como Chart. ToWebImage, Chart. Save y Chart. Write. |
| Criptografía | Utiliza algoritmos hash para crear contraseñas con valores Salt y hash correctamente. |
| WebGrid | Representa una colección de objetos (normalmente, datos de una base de datos) como una cuadrícula. Admite la paginación y la ordenación. |
| WebImage | Representa una imagen. |
| Correo web | Envía un mensaje de correo electrónico. |

Un tema de referencia rápida que enumera las aplicaciones auxiliares y la sintaxis básica está disponible como parte de la documentación de ASP.NET sintaxis Razor en la siguiente dirección URL:

[https://www.asp.net/webmatrix/tutorials/asp-net-web-pages-api-reference](../web-pages/overview/api-reference/asp-net-web-pages-api-reference.md)

### <a id="0.1__Toc274034220"></a>Compatibilidad adicional con la inserción de dependencias

A partir de la versión de ASP.NET MVC 3 Preview 1, la versión actual incluye compatibilidad agregada para dos nuevos servicios y cuatro servicios existentes, y compatibilidad mejorada para la resolución de dependencias y el localizador de servicios común.

#### <a name="new-icontrolleractivator-interface-for-fine-grained-controller-instantiation"></a>Nueva interfaz IControllerActivator para la creación de instancias de controlador específica

La nueva interfaz IControllerActivator proporciona un control más preciso sobre cómo se crean instancias de los controladores mediante la inserción de dependencias. En el ejemplo siguiente se muestra la interfaz:

[!code-csharp[Main](mvc3-release-notes/samples/sample27.cs)]

Compare esto con el rol del generador de controladores. Un generador de controladores es una implementación de la interfaz IControllerFactory, que es responsable de buscar un tipo de controlador y de crear una instancia de ese tipo de controlador.

Los activadores de controlador solo son responsables de crear instancias de una instancia de un tipo de controlador. No realizan la búsqueda del tipo de controlador. Después de encontrar un tipo de controlador adecuado, los generadores de controladores deben delegar en una instancia de IControllerActivator para controlar la creación de instancias real del controlador.

La clase DefaultControllerFactory tiene un nuevo constructor que acepta una instancia de IControllerFactory. Esto le permite aplicar la inserción de dependencias para administrar este aspecto de la creación del controlador sin tener que invalidar el comportamiento predeterminado de búsqueda del tipo de controlador.

#### <a name="iservicelocator-interface-replaced-with-idependencyresolver"></a>La interfaz Iservicelocator del se reemplaza por objeto idependencyresolver

En función de los comentarios de la comunidad, la versión beta de ASP.NET MVC 3 ha reemplazado el uso de la interfaz Iservicelocator del por una interfaz objeto idependencyresolver de versión reducida-Down específica para las necesidades de ASP.NET MVC. En el ejemplo siguiente se muestra la nueva interfaz:

[!code-csharp[Main](mvc3-release-notes/samples/sample28.cs)]

Como parte de este cambio, la clase ServiceLocator también se ha reemplazado por la clase DependencyResolver. El registro de una resolución de dependencias es similar a las versiones anteriores de ASP.NET MVC:

[!code-csharp[Main](mvc3-release-notes/samples/sample29.cs)]

Las implementaciones de esta interfaz simplemente deben delegar en el contenedor de inserción de dependencias subyacente para proporcionar el servicio registrado para el tipo solicitado.

Cuando no hay ningún servicio registrado del tipo solicitado, ASP.NET MVC espera implementaciones de esta interfaz para devolver null desde GetService y devolver una colección vacía de GetServices.

La nueva clase DependencyResolver permite registrar clases que implementan la nueva interfaz objeto idependencyresolver o la interfaz del localizador de servicios común (Iservicelocator del). Para obtener más información sobre el localizador de servicios común, consulte [CommonServiceLocator en github](https://github.com/unitycontainer/commonservicelocator).

<a id="0.1__Breaking_Changes"></a>

#### <a name="new-iviewactivator-interface-for-fine-grained-view-page-instantiation"></a>Nueva interfaz IViewActivator para la creación de instancias de la página de vista específica

La nueva interfaz IViewPageActivator proporciona un control más preciso sobre cómo se crean instancias de las páginas de vistas a través de la inserción de dependencias. Esto se aplica tanto a instancias de WebFormView como a instancias de RazorView. En el ejemplo siguiente se muestra la nueva interfaz:

[!code-csharp[Main](mvc3-release-notes/samples/sample30.cs)]

Estas clases ahora aceptan un argumento de constructor IViewPageActivator, que permite usar la inserción de dependencias para controlar cómo se crean instancias de los tipos ViewPage, ViewUserControl y WebViewPage.

#### <a name="new-dependency-resolver-support-for-existing-services"></a>Nueva compatibilidad con la resolución de dependencias para los servicios existentes

La nueva versión incluye compatibilidad con la resolución de dependencias para los siguientes servicios:

- Proveedores de validación de modelos. Las clases que implementan ModelValidatorProvider se pueden registrar en la resolución de dependencias y el sistema las usará para admitir la validación del lado cliente y del servidor.
- Proveedor de metadatos del modelo. Una única clase que implementa ModelMetadataProvider se puede registrar en la resolución de dependencias y el sistema la usará para proporcionar metadatos para los sistemas de validación y de plantillas.
- Proveedores de valores. Las clases que implementan ValueProviderFactory se pueden registrar en la resolución de dependencias y el sistema las usará para crear proveedores de valores que el controlador utiliza y durante el enlace de modelos.
- Enlazadores de modelos. Las clases que implementan IModelBinderProvider se pueden registrar en la resolución de dependencias y el sistema las usará para crear enlazadores de modelos utilizados por el sistema de enlace de modelos.

### <a id="0.1__Toc274034221"></a>Nueva compatibilidad con Ajax no intrusivo basado en jQuery

ASP.NET MVC incluye métodos auxiliares de Ajax como los siguientes:

- Ajax.ActionLink
- Ajax.RouteLink
- Ajax.BeginForm
- Ajax.BeginRouteForm

Estos métodos usan JavaScript para invocar un método de acción en el servidor en lugar de usar un postback completo. Esta funcionalidad se ha actualizado para aprovechar las ventajas de jQuery de manera discreta. En lugar de emitir de forma intrusiva scripts de cliente en línea, estos métodos auxiliares separan el comportamiento del marcado mediante la emisión de atributos HTML5 mediante el prefijo de *AJAX de datos* . A continuación, se aplica el comportamiento al marcado haciendo referencia a los archivos de JavaScript adecuados. Asegúrese de que se hace referencia a los siguientes archivos JavaScript:

- jquery-1.4.1.js
- jquery.unobtrusive.ajax.js

Esta característica está habilitada de forma predeterminada en el archivo Web. config en las nuevas plantillas de proyecto de ASP.NET MVC 3, pero está deshabilitada de forma predeterminada para los proyectos existentes. Para obtener más información, consulte [Agregar marcas para toda la aplicación para la validación del cliente y JavaScript discreto](#0.1_AddedApplicationWideFlagsForClientValida) más adelante en este documento.

### <a id="0.1__Toc274034222"></a>Nueva compatibilidad con la validación de jQuery discreta

De forma predeterminada, ASP.NET MVC 3 beta usa la validación de jQuery de forma discreta para realizar la validación del lado cliente. Para habilitar la validación discreta del cliente, realice una llamada como la siguiente desde una vista:

[!code-csharp[Main](mvc3-release-notes/samples/sample31.cs)]

Esto requiere que la propiedad ViewContext. UnobtrusiveJavaScriptEnabled esté establecida en true, lo que puede hacer si realiza la llamada siguiente:

[!code-csharp[Main](mvc3-release-notes/samples/sample32.cs)]

Asegúrese también de que se hace referencia a los siguientes archivos JavaScript.

- jquery-1.4.1.js
- jquery.validate.js
- jquery.validate.unobtrusive.js

Esta característica está habilitada de forma predeterminada en el archivo Web. config en las nuevas plantillas de proyecto de ASP.NET MVC 3, pero está deshabilitada de forma predeterminada para los proyectos existentes. Para obtener más información, consulte [nuevas marcas para toda la aplicación para la validación de clientes y JavaScript discreto](#0.1_AddedApplicationWideFlagsForClientValida) más adelante en este documento.

<a id="0.1__Toc274034223"></a>

### <a id="0.1_AddedApplicationWideFlagsForClientValida"></a>Nuevas marcas de toda la aplicación para la validación de clientes y JavaScript discreto

Puede habilitar o deshabilitar la validación de cliente y el JavaScript discreto globalmente mediante miembros estáticos de la clase HtmlHelper, como en el ejemplo siguiente:

[!code-csharp[Main](mvc3-release-notes/samples/sample33.cs)]

De forma predeterminada, las plantillas de proyecto predeterminadas habilitan JavaScript discreto. También puede habilitar o deshabilitar estas características en el archivo raíz Web. config de la aplicación con la siguiente configuración:

[!code-xml[Main](mvc3-release-notes/samples/sample34.xml)]

Dado que puede habilitar estas características de forma predeterminada, se introdujeron nuevas sobrecargas en la clase HtmlHelper que permiten invalidar la configuración predeterminada, como se muestra en los ejemplos siguientes:

[!code-csharp[Main](mvc3-release-notes/samples/sample35.cs)]

Por compatibilidad con versiones anteriores, ambas características están deshabilitadas de forma predeterminada.

### <a id="0.1__Toc274034224"></a>Nueva compatibilidad con el código que se ejecuta antes de que se ejecuten las vistas

Ahora puede colocar un archivo llamado \_viewstart. cshtml (o \_viewstart. vbhtml) en el directorio views y agregarle código que se compartirá entre varias vistas de ese directorio y sus subdirectorios. Por ejemplo, puede colocar el código siguiente en la página \_viewstart. cshtml de la carpeta ~/views:

[!code-cshtml[Main](mvc3-release-notes/samples/sample36.cshtml)]

Esto establece la página de diseño de cada vista de la carpeta views y de todas sus subcarpetas de forma recursiva. Cuando se representa una vista, el código del archivo \_viewstart. cshtml se ejecuta antes de que se ejecute el código de vista. El código \_viewstart. cshtml se aplica a cada vista de esa carpeta.

De forma predeterminada, el código del archivo \_viewstart. cshtml también se aplica a las vistas de cualquier subcarpeta. Sin embargo, las subcarpetas individuales pueden tener su propia versión del archivo \_viewstart. cshtml; en ese caso, la versión local tiene prioridad. Por ejemplo, para ejecutar código que es común a todas las vistas del HomeController, coloque un \_archivo viewstart. cshtml en la carpeta ~/Views/Home.

### <a id="0.1__Toc274034225"></a>Nueva compatibilidad con la sintaxis de Razor de VBHTML

La versión preliminar de ASP.NET MVC anterior incluía compatibilidad con vistas que C#usan sintaxis Razor basado en. Estas vistas usan la extensión de archivo. cshtml. Como parte del trabajo en curso para admitir Razor, la versión beta de ASP.NET MVC 3 presenta compatibilidad con el sintaxis Razor en Visual Basic, que usa la extensión de archivo. vbhtml.

Para obtener una introducción al uso de Visual Basic sintaxis en las páginas de VBHTML, vea el tutorial en la dirección URL siguiente:

[https://www.asp.net/webmatrix/tutorials/asp-net-web-pages-visual-basic](../web-pages/overview/getting-started/introducing-razor-syntax-vb.md)

### <a id="0.1__Toc274034226"></a>Control más granular sobre ValidateInputAttribute

ASP.NET MVC siempre ha incluido la clase ValidateInputAttribute, que invoca la infraestructura de validación de solicitudes Core ASP.NET para asegurarse de que la solicitud entrante no contiene entradas potencialmente malintencionadas. De forma predeterminada, la validación de entrada está habilitada. Es posible deshabilitar la validación de solicitudes mediante el atributo ValidateInputAttribute, como en el ejemplo siguiente:

[!code-csharp[Main](mvc3-release-notes/samples/sample37.cs)]

Sin embargo, muchas aplicaciones web tienen campos de formulario individuales que necesitan permitir HTML, mientras que los campos restantes no deberían. La clase ValidateInputAttribute ahora permite especificar una lista de campos que no deben incluirse en la validación de solicitudes.

Por ejemplo, si está desarrollando un motor de blog, puede que desee permitir el marcado en los campos cuerpo y Resumen. Estos campos pueden estar representados por dos elementos Input, cada uno con un atributo name que corresponde al nombre de la propiedad ("Body" y "Summary"). Para deshabilitar la validación de solicitudes solo para estos campos, especifique los nombres (separados por comas) en la propiedad exclude de la clase ValidateInput, como en el ejemplo siguiente:

[!code-csharp[Main](mvc3-release-notes/samples/sample38.cs)]

### <a id="0.1__Toc274034227"></a>Las aplicaciones auxiliares convierten el carácter de subrayado en guiones para los nombres de atributos HTML especificados mediante objetos anónimos

Los métodos auxiliares permiten especificar pares de nombre/valor de atributo mediante un objeto anónimo, como en el ejemplo siguiente:

[!code-csharp[Main](mvc3-release-notes/samples/sample39.cs)]

Este enfoque no permite usar guiones en el nombre del atributo, ya que no se puede usar un guion para un nombre de propiedad en ASP.NET. Sin embargo, los guiones son importantes para los atributos HTML5 personalizados; por ejemplo, HTML5 usa el prefijo "Data-".

Al mismo tiempo, los guiones bajos no se pueden usar para los nombres de atributo en HTML, sino que son válidos en los nombres de propiedad. Por lo tanto, si especifica atributos mediante un objeto anónimo y los nombres de atributo incluyen un carácter de subrayado,, los métodos auxiliares convertirán los guiones bajos en guiones. Por ejemplo, la siguiente sintaxis de la aplicación auxiliar utiliza un carácter de subrayado:

[!code-csharp[Main](mvc3-release-notes/samples/sample40.cs)]

En el ejemplo anterior se representa el siguiente marcado cuando se ejecuta la aplicación auxiliar:

[!code-html[Main](mvc3-release-notes/samples/sample41.html)]

## <a id="0.1__Toc274034228"></a>Correcciones de errores

La plantilla de objeto predeterminada para las aplicaciones auxiliares de plantilla EditorFor y DisplayFor ahora admite la ordenación especificada en la propiedad DisplayAttribute. order. (En versiones anteriores, no se usaba la configuración de orden).

La validación de cliente ahora admite la validación de propiedades invalidadas que tienen atributos de validación aplicados.

JsonValueProviderFactory se registra ahora de forma predeterminada.

## <a id="0.1__Toc274034229"></a>Cambios importantes

El orden de ejecución de los filtros de excepciones ha cambiado en los filtros de excepciones que tienen el mismo valor de orden. En ASP.NET MVC 2 y versiones anteriores, los filtros de excepción en el controlador con el mismo orden que los de un método de acción se ejecutaban antes que los filtros de excepción en el método de acción. Este suele ser el caso cuando se aplican filtros de excepción sin un valor de orden especificado. En ASP.NET MVC 3, este orden se ha invertido para que el controlador de excepciones más específico se ejecute primero. Como en versiones anteriores, si se especifica explícitamente la propiedad Order, los filtros se ejecutan en el orden especificado.

## <a id="0.1__Toc274034230"></a>  Problemas conocidos

Durante la instalación, el cuadro de diálogo aceptación del CLUF muestra los términos de la licencia en una ventana menor de lo previsto.

Las vistas de Razor no tienen compatibilidad con IntelliSense ni resaltado de sintaxis. Se prevé que la compatibilidad con sintaxis Razor en Visual Studio se incluirá como parte de una versión posterior.

Al editar una vista de Razor (archivo CSHTML), el <a id="0.1__Toc224729061"></a> <a id="0.1__Toc238051347"></a> elemento de menú ir a controlador de Visual Studio no estará disponible y no habrá fragmentos de código.

Cuando se usa la sintaxis de @model para especificar una vista CSHTML fuertemente tipada, no se reconocen los métodos abreviados específicos del lenguaje para los tipos. Por ejemplo, @model int no funcionará, pero @model Int32 funcionará. La solución para este error es usar el nombre de tipo real al especificar el tipo de modelo.

Cuando se usa la sintaxis de @model para especificar una vista CSHTML fuertemente tipada (o @ModelType para especificar una vista VBHTML fuertemente tipada), no se admiten tipos que aceptan valores NULL ni declaraciones de matriz. Por ejemplo, @model int? no se admite. En su lugar, use `@model Nullable<Int32>`. Tampoco se admite la sintaxis @model String []; en su lugar, use `@model IList<string>`.

Al actualizar un proyecto de ASP.NET MVC 2 a ASP.NET MVC 3, asegúrese de agregar lo siguiente a la sección appSettings del archivo Web. config:

[!code-xml[Main](mvc3-release-notes/samples/sample42.xml)]

Hay un problema conocido que hace que la autenticación de formularios redirija siempre a los usuarios no autenticados a ~/Account/Login, omitiendo la configuración de autenticación de formularios usada en Web. config. La solución consiste en agregar la siguiente configuración de aplicación.

[!code-xml[Main](mvc3-release-notes/samples/sample43.xml)]

## <a id="0.1__Toc274034231"></a>Ausencia

© 2011 Microsoft Corporation. All rights reserved. Este documento se proporciona "tal cual". La información y las opiniones expresadas en este documento, como las direcciones URL y otras referencias a sitios web de Internet, pueden cambiar sin previo aviso. Usted asume el riesgo de utilizarla.

Este documento no le proporciona ningún derecho legal sobre ninguna propiedad intelectual de ningún producto de Microsoft. Puede copiar y usar este documento para su uso interno de referencia.
