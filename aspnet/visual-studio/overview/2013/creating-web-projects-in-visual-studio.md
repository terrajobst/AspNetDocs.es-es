---
uid: visual-studio/overview/2013/creating-web-projects-in-visual-studio
title: Crear proyectos Web de ASP.NET en Visual Studio 2013 | Microsoft Docs
author: tdykstra
description: En este tema se explican las opciones para crear proyectos Web de ASP.NET en Visual Studio 2013 con Update 3, estas son algunas de las nuevas características para el desarrollo web c...
ms.author: riande
ms.date: 12/01/2014
ms.assetid: 61941e64-0c0d-4996-9270-cb8ccfd0cabc
msc.legacyurl: /visual-studio/overview/2013/creating-web-projects-in-visual-studio
msc.type: authoredcontent
ms.openlocfilehash: fbb4cd7afa2506879d47bce980bf0164aad40c2c
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78447205"
---
# <a name="creating-aspnet-web-projects-in-visual-studio-2013"></a>Crear proyectos web de ASP.NET en Visual Studio 2013

por [Tom Dykstra](https://github.com/tdykstra)

> En este tema se explican las opciones para crear proyectos Web de ASP.NET en Visual Studio 2013 con Update 3.
> 
> Estas son algunas de las nuevas características para el desarrollo web en comparación con las versiones anteriores de Visual Studio:
> 
> - Una interfaz de usuario sencilla para crear proyectos que ofrecen [compatibilidad con varios marcos de ASP.net](#add) (Web Forms, MVC y Web API).
> - [ASP.net Identity](#indauth), un nuevo sistema de pertenencia a ASP.net que funciona igual en todos los marcos de ASP.net y funciona con software de hospedaje web que no sea IIS.
> - El uso de [bootstrap](#bootstrap) para proporcionar capacidades de diseño y de la capacidad de respuesta.
> - Nuevas características para formularios Web Forms que solían ofrecerse para MVC, como la [creación de proyectos de prueba automática](#testproj) y una [plantilla de sitio de intranet](#winauth).
> 
> Para obtener información sobre cómo crear proyectos web para Azure Cloud Services o Azure Mobile Services, consulte Introducción [a azure Cloud Services y ASP.net](https://azure.microsoft.com/documentation/articles/cloud-services-dotnet-get-started/) y [creación de una aplicación de marcador con el back-end de Azure Mobile Services .net](https://azure.microsoft.com/documentation/articles/mobile-services-dotnet-backend-windows-store-dotnet-leaderboard/).

<a id="prerequisites"></a>
## <a name="prerequisites"></a>Requisitos previos

Este artículo se aplica a [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566) con [Update 3](https://go.microsoft.com/fwlink/?linkid=397827&amp;clcid=0x409) instalado.

<a id="wap"></a>
## <a name="web-application-projects-versus-web-site-projects"></a>Proyectos de aplicación web frente a proyectos de sitio web

ASP.NET le permite elegir entre dos tipos de proyectos web: *proyectos de aplicación web* y *proyectos de sitio web*. Se recomiendan proyectos de aplicación web para el nuevo desarrollo y este artículo solo se aplica a los proyectos de aplicación Web. Para obtener más información, vea [proyectos de aplicación web frente a proyectos de sitio web en Visual Studio](https://msdn.microsoft.com/library/dd547590(v=vs.120).aspx) en el sitio de MSDN.

<a id="overview"></a>
## <a name="overview-of-web-application-project-creation"></a>Información general sobre la creación de proyectos de aplicación Web

En los pasos siguientes se muestra cómo crear un proyecto web:

1. Haga clic en **nuevo proyecto** en la página **Inicio** o en el menú **archivo** .
2. En el cuadro de diálogo **nuevo proyecto** , haga clic en **Web** en el panel izquierdo y **ASP.net aplicación web** en el panel central.

    ![Cuadro de diálogo Nuevo proyecto](creating-web-projects-in-visual-studio/_static/image1.png)

    Puede elegir **nube** en el panel izquierdo para crear un servicio en la [nube de Azure](https://docs.microsoft.com/azure/cloud-services/cloud-services-how-to-create-deploy), un [servicio móvil de Azure](https://msdn.microsoft.com/library/windows/apps/dn629482.aspx)o un WebJob de [Azure](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-webjobs). En este tema no se tratan esas plantillas.
3. En el panel derecho, haga clic en la casilla **agregar Application Insights a proyecto** si desea supervisar el estado y el uso de la aplicación. Para más información, consulte [Supervisar el rendimiento de aplicaciones web](https://azure.microsoft.com/documentation/articles/app-insights-web-monitor-performance/).
4. Especifique **el nombre**del proyecto, la **Ubicación**y otras opciones y, a continuación, haga clic en **Aceptar**.

    Aparece el cuadro de diálogo **Nuevo proyecto ASP.NET**.

    ![Cuadro de diálogo Nuevo proyecto](creating-web-projects-in-visual-studio/_static/image2.png)
5. Haga clic en una plantilla.

    ![Seleccione una plantilla:](creating-web-projects-in-visual-studio/_static/image3.png)
6. Si desea agregar compatibilidad con marcos adicionales que no están incluidos en la plantilla, haga clic en la casilla correspondiente. (En el ejemplo que se muestra, puede Agregar MVC y/o API Web a un proyecto de formularios Web Forms).

    ![Agregar marcos de trabajo](creating-web-projects-in-visual-studio/_static/image4.png)
7. <a id="testproj"></a>Si desea agregar un proyecto de prueba unitaria, haga clic en **Agregar pruebas unitarias**.

    ![Adición de pruebas unitarias](creating-web-projects-in-visual-studio/_static/image5.png)
8. Si desea un método de autenticación diferente del que proporciona la plantilla de forma predeterminada, haga clic en **cambiar autenticación**.

    ![Botón Configurar autenticación](creating-web-projects-in-visual-studio/_static/image6.png)

    ![Cuadro de diálogo Configurar autenticación](creating-web-projects-in-visual-studio/_static/image7.png)

<a id="azurenewproj"></a>
### <a name="create-a-web-app-or-virtual-machine-in-azure"></a>Creación de una aplicación web o una máquina virtual en Azure

Visual Studio incluye características que facilitan el trabajo con los servicios de Azure para hospedar aplicaciones Web. Por ejemplo, puede hacer lo siguiente directamente desde el IDE de Visual Studio:

- Cree y administre aplicaciones web o máquinas virtuales que hagan que la aplicación esté disponible a través de Internet.
- Ver los registros creados por la aplicación cuando se ejecuta en la nube.
- Ejecutar en modo de depuración de forma remota mientras la aplicación se ejecuta en la nube.
- Vea y administre otros servicios de Azure, como las bases de datos SQL.

Puede [crear una cuenta de Azure](https://www.windowsazure.com/pricing/free-trial/) que incluya servicios básicos como Web Apps gratis y, si es un suscriptor de MSDN, puede activar las [ventajas](https://azure.microsoft.com/pricing/member-offers/visual-studio-subscriptions/) que le ofrecen créditos mensuales para servicios adicionales de Azure. 

De forma predeterminada, el cuadro de diálogo **nuevo proyecto ASP.net** le permite crear una aplicación web o una máquina virtual para un nuevo proyecto Web. Si no desea crear una nueva aplicación web o máquina virtual, desactive la casilla **hospedar en la nube** .

![Crear recursos remotos](creating-web-projects-in-visual-studio/_static/image8.png)

El título de la casilla podría ser **hospedado en la nube** o **crear recursos remotos**y, en cualquier caso, el efecto es el mismo. Si deja activada la casilla, Visual Studio crea una aplicación web en Azure App Service de forma predeterminada. Puede usar el cuadro desplegable para cambiarlo a una **máquina virtual** si lo prefiere. Si aún no ha iniciado sesión en Azure, se le pedirán las credenciales de Azure. Después de iniciar sesión, un cuadro de diálogo le permite configurar los recursos que Visual Studio creará para el proyecto. En la ilustración siguiente se muestra el cuadro de diálogo para una aplicación Web; Si decide crear una máquina virtual, aparecerán distintas opciones.

![Configurar los valores de App de Azure](creating-web-projects-in-visual-studio/_static/image9.png)

Para obtener más información sobre cómo usar este proceso para crear recursos de Azure, consulte Introducción [a Azure y ASP.net](https://docs.microsoft.com/azure/app-service-web/app-service-web-get-started-dotnet) y [creación de una máquina virtual para un sitio web con Visual Studio](https://azure.microsoft.com/documentation/articles/virtual-machines-dotnet-create-visual-studio-powershell/).

En el resto de este artículo se proporciona más información acerca de las plantillas disponibles y sus opciones. En el artículo también se presenta Bootstrap, el diseño y los marcos de trabajo que se usan en las plantillas.

<a id="vs2013"></a>
## <a name="visual-studio-2013-web-project-templates"></a>Visual Studio 2013 plantillas de proyecto web

Visual Studio usa plantillas para crear proyectos Web. Una plantilla de proyecto puede crear archivos y carpetas en el nuevo proyecto, instalar paquetes NuGet y proporcionar código de ejemplo para una aplicación de trabajo rudimentaria. Las plantillas implementan los estándares web más recientes y se han diseñado para demostrar las prácticas recomendadas para usar las tecnologías de ASP.NET, además de ofrecer una introducción a la creación de su propia aplicación.

Visual Studio 2013 proporciona las siguientes opciones para las plantillas de proyecto web para los proyectos que tienen como destino .NET 4,5 o versiones posteriores de .NET Framework:

- [Plantilla vacía](#empty)
- [Plantilla de formularios Web Forms](#wf)
- [Plantilla MVC](#mvc)
- [Plantilla de API Web](#webapi)
- [Plantilla de aplicación de una sola página](#spa)
- [Plantilla de servicio móvil de Azure](https://azure.microsoft.com/documentation/articles/mobile-services-dotnet-backend-windows-store-dotnet-leaderboard/)
- [Plantillas de Visual Studio 2012](#vs2012)

También puede instalar una extensión de Visual Studio que proporciona una [plantilla de Facebook](#facebook).

Para obtener información sobre cómo crear proyectos que tengan como destino .NET 4, consulte [plantillas de Visual Studio 2012](#vs2012) más adelante en este tema.

Para obtener información sobre cómo crear aplicaciones de ASP.NET para clientes móviles, consulte [compatibilidad con dispositivos móviles en ASP.net](../../../mobile/overview.md).

<a id="empty"></a>
### <a name="empty-template"></a>Plantilla vacía

La plantilla vacía proporciona el número mínimo de carpetas y archivos para una aplicación Web de ASP.NET, como un archivo de proyecto ( *. csproj* o. *vbproj*) y un archivo *Web. config* . Puede Agregar compatibilidad con formularios Web Forms, MVC y/o API Web mediante las casillas de la etiqueta **Agregar carpetas y referencias principales para:** .

Para la plantilla vacía, no hay opciones de autenticación disponibles. La funcionalidad de autenticación se implementa en aplicaciones de ejemplo y la plantilla vacía no crea una aplicación de ejemplo.

<a id="wf"></a>
### <a name="web-forms-template"></a>Plantilla de formularios Web Forms

El marco de trabajo de formularios Web Forms proporciona las siguientes características que permiten crear rápidamente sitios web que son ricos en características de acceso a datos y interfaz de usuario:

- Un diseñador WYSIWYG en Visual Studio.
- Controles de servidor que representan HTML y que se pueden personalizar estableciendo propiedades y estilos.
- Gran variedad de controles para el acceso a datos y la presentación de datos.
- Modelo de eventos que expone eventos que se pueden programar como si se programara una aplicación cliente como WPF.
- Preservación automática del estado (datos) entre las solicitudes HTTP.

En general, la creación de una aplicación de formularios Web Forms requiere menos esfuerzo de programación que la creación de la misma aplicación mediante el marco ASP.NET MVC. Sin embargo, Web Forms no es solo para el desarrollo rápido de aplicaciones. Hay muchas aplicaciones y marcos de trabajo comerciales complejos basados en formularios Web Forms.

Dado que una página de formularios Web Forms y los controles de la página generan automáticamente gran parte del marcado que se envía al explorador, no tiene el tipo de control preciso sobre el HTML que ASP.NET MVC ofrece. El modelo declarativo para configurar páginas y controles minimiza la cantidad de código que se debe escribir, pero oculta parte del comportamiento de HTML y HTTP. Por ejemplo, no siempre es posible especificar exactamente qué marcado podría generar un control.

El marco de trabajo de formularios Web Forms no se presta tan bien como ASP.NET MVC a prácticas de desarrollo basadas en patrones como el [desarrollo controlado por pruebas](http://en.wikipedia.org/wiki/Test-driven_development), la [separación de preocupaciones](http://en.wikipedia.org/wiki/Separation_of_concerns), la [inversión de control](http://en.wikipedia.org/wiki/Inversion_of_control)y la [inserción de dependencias](http://en.wikipedia.org/wiki/Dependency_injection). Si desea escribir código factorizado de esta manera, puede; no es tan automático como está en el marco de ASP.NET MVC. El proyecto [MVP de formularios Web Forms de ASP.net](http://webformsmvp.com/) muestra un enfoque que facilita la separación de preocupaciones y la capacidad de prueba, a la vez que mantiene el desarrollo rápido que los formularios Web Forms se crearon para ofrecer. Microsoft SharePoint se basa en MVP de formularios Web Forms.

La plantilla de formularios Web Forms crea una aplicación de formularios Web Forms de ejemplo que usa [bootstrap](#bootstrap) para proporcionar características de diseño y de creación de funciones con capacidad de respuesta. En la ilustración siguiente se muestra la Página principal.

![Página principal de la aplicación de plantilla de formularios Web Forms](creating-web-projects-in-visual-studio/_static/image10.png)

Para obtener más información sobre los formularios Web Forms, vea [ASP.NET Web Forms](https://asp.net/web-forms). Para obtener información acerca de lo que hace la plantilla de formularios Web Forms, vea [compilar una aplicación básica de formularios Web Forms mediante Visual Studio 2013](https://blogs.msdn.com/b/webdev/archive/2013/12/19/building-a-basic-web-forms-application-using-visual-studio-2013.aspx).

<a id="mvc"></a>
### <a name="mvc-template"></a>Plantilla MVC

ASP.NET MVC se diseñó para facilitar prácticas de desarrollo basadas en patrones como el [desarrollo controlado por pruebas](http://en.wikipedia.org/wiki/Test-driven_development), la [separación de preocupaciones](http://en.wikipedia.org/wiki/Separation_of_concerns), la [inversión de control](http://en.wikipedia.org/wiki/Inversion_of_control)y la [inserción de dependencias](http://en.wikipedia.org/wiki/Dependency_injection). El marco de trabajo fomenta separar el nivel de lógica de negocios de una aplicación web del nivel de presentación. Al dividir la aplicación en modelos (M), vistas (V) y controladores (C), ASP.NET MVC puede facilitar la administración de la complejidad en aplicaciones más grandes.

Con ASP.NET MVC, puede trabajar directamente con HTML y HTTP que en formularios Web Forms. Por ejemplo, los formularios Web Forms pueden conservar automáticamente el estado entre las solicitudes HTTP, pero tiene que codificarlo explícitamente en MVC. La ventaja del modelo de MVC es que permite tener un control completo sobre lo que hace la aplicación y cómo se comporta en el entorno Web. El inconveniente es que tiene que escribir más código.

MVC se diseñó para ser extensible, lo que proporciona a los desarrolladores de energía la capacidad de personalizar el marco de trabajo para sus necesidades de aplicación. Además, el código fuente de MVC de ASP.NET está disponible en una licencia OSI.

La plantilla MVC crea una aplicación de MVC 5 de ejemplo que usa [bootstrap](#bootstrap) para proporcionar características de diseño y de creación de funciones con capacidad de respuesta. En la ilustración siguiente se muestra la Página principal.

![Aplicación de ejemplo MVC](creating-web-projects-in-visual-studio/_static/image11.png)

Para obtener más información sobre MVC, vea [ASP.NET MVC](https://asp.net/mvc). Para obtener información acerca de cómo seleccionar la plantilla MVC 4, consulte [plantillas de Visual Studio 2012](#vs2012) más adelante en este artículo.

<a id="webapi"></a>
### <a name="web-api-template"></a>Plantilla de API Web

La plantilla de API Web crea un servicio Web de ejemplo basado en Web API, incluidas las páginas de ayuda de API basadas en MVC.

ASP.NET Web API es un marco que facilita la creación de servicios HTTP disponibles para una amplia variedad de clientes, entre los que se incluyen exploradores y dispositivos móviles. ASP.NET Web API es una plataforma ideal para la creación de servicios RESTful en el .NET Framework.

La plantilla de API Web crea un servicio Web de ejemplo. En las ilustraciones siguientes se muestran páginas de ayuda de ejemplo.

![Página de ayuda de Web API](creating-web-projects-in-visual-studio/_static/image12.png)

![Página de ayuda de la API Web para GET API](creating-web-projects-in-visual-studio/_static/image13.png)

Para obtener más información sobre la API Web, consulte [ASP.net web API](https://asp.net/web-api).

<a id="spa"></a>
### <a name="single-page-application-template"></a>Plantilla de aplicación de página única

La plantilla de aplicación de una sola página (SPA) crea una aplicación de ejemplo que usa JavaScript, HTML 5 y [KnockoutJS](http://knockoutjs.com/) en el cliente, y ASP.net web API en el servidor.

La única opción de autenticación para la plantilla SPA es [cuentas de usuario individuales](#indauth).

En la ilustración siguiente se muestra el estado inicial de la aplicación de ejemplo que compila la plantilla SPA.

![Aplicación de ejemplo de SPA](creating-web-projects-in-visual-studio/_static/image14.png)

Para obtener información sobre cómo crear una aplicación con la plantilla de SPA, consulte [API Web-servicios de autenticación externos](../../../web-api/overview/security/external-authentication-services.md).

Para obtener más información sobre las aplicaciones de una sola página de ASP.NET y sobre plantillas de SPA adicionales que usan marcos de JavaScript distintos de KnockoutJS, consulte los siguientes recursos:

- [Aplicación de una sola página de ASP.net](../../../single-page-application/index.md).
- [Descripción de las características de seguridad de la plantilla de SPA para VS2013 RC](https://blogs.msdn.com/b/webdev/archive/2013/09/20/understanding-security-features-in-spa-template.aspx)
- [Aplicaciones de una sola página: cree Web Apps actuales y con capacidad de respuesta con ASP.NET](https://msdn.microsoft.com/magazine/dn463786.aspx)

<a id="facebook"></a>
### <a name="facebook-template"></a>Plantilla de Facebook

Puede instalar una [extensión de Visual Studio que proporciona una plantilla de Facebook](https://go.microsoft.com/fwlink/?LinkID=509965&amp;clcid=0x409). Esta plantilla crea una aplicación de ejemplo diseñada para ejecutarse en el sitio web de Facebook. Se basa en ASP.NET MVC y usa la API Web para la funcionalidad de actualización en tiempo real.

No hay opciones de autenticación disponibles para la plantilla de Facebook porque las aplicaciones de Facebook se ejecutan en el sitio de Facebook y se basan en la autenticación de Facebook.

Para obtener más información sobre las aplicaciones de ASP.NET Facebook, consulte [actualización de la API de Facebook para MVC](https://blogs.msdn.com/b/webdev/archive/2014/06/10/updating-the-mvc-facebook-api.aspx).

<a id="vs2012"></a>
### <a name="visual-studio-2012-templates"></a>Plantillas de Visual Studio 2012

El cuadro de diálogo Visual Studio 2013 creación de proyectos web no proporciona acceso a algunas plantillas que estaban disponibles en Visual Studio 2012. Si desea usar una de estas plantillas, puede hacer clic en el nodo Visual Studio 2012 en el panel izquierdo del cuadro de diálogo nuevo proyecto de Visual Studio.

![Plantillas de Visual Studio 2012](creating-web-projects-in-visual-studio/_static/image15.png)

El nodo de **Visual Studio 2012** le permite elegir las siguientes plantillas web a las que no tiene acceso en la lista predeterminada de plantillas para Visual Studio 2013:

- Aplicación web MVC 4 de ASP.NET
- Aplicación web de Entidades de datos dinámicos de ASP.NET
- Control de servidor de AJAX de ASP.NET
- Extensor de control de servidor de ASP.NET AJAX
- Control de servidor ASP.NET

<a id="bootstrap"></a>
## <a name="bootstrap-in-the-visual-studio-2013-web-project-templates"></a>Bootstrap en el Visual Studio 2013 plantillas de proyecto web

Las plantillas de proyecto de Visual Studio 2013 usan [bootstrap](http://getbootstrap.com/), un diseño y un marco de trabajo de creación de proyectos creados por Twitter. Bootstrap usa CSS3 para proporcionar un diseño dinámico, lo que significa que los diseños pueden adaptarse dinámicamente a distintos tamaños de ventana del explorador. Por ejemplo, en una ventana de explorador ancha, la Página principal creada por la plantilla de formularios Web Forms tiene un aspecto similar al de la siguiente ilustración:

![Página principal de la aplicación de plantilla de formularios Web Forms](creating-web-projects-in-visual-studio/_static/image16.png)

Haga que la ventana sea más estrecha y que las columnas organizadas horizontalmente pasen a la disposición vertical:

![Disposición de columnas vertical de bootstrap](creating-web-projects-in-visual-studio/_static/image17.png)

Restrinja la ventana un poco más y el menú superior horizontal se convierte en un icono en el que puede hacer clic para expandirse en un menú orientado verticalmente:

![Icono de menú de arranque](creating-web-projects-in-visual-studio/_static/image18.png)

![Menú de arranque vertical](creating-web-projects-in-visual-studio/_static/image19.png)

También puede usar la característica de las funciones de bootstrap para aplicar fácilmente un cambio en la apariencia y el funcionamiento de la aplicación. Por ejemplo, puede realizar los pasos siguientes para cambiar el tema.

1. En el explorador, vaya a [http://Bootswatch.com](http://Bootswatch.com), elija un tema y, a continuación, haga clic en **Descargar**. (Esto descarga *bootstrap. min. CSS* de forma predeterminada; si desea examinar el código CSS, obtenga *bootstrap. CSS* en lugar de la versión reducida).
2. Copie el contenido del archivo CSS descargado.
3. En Visual Studio, cree un nuevo archivo de **hoja de estilos** denominado *bootstrap-Theme. CSS* en la carpeta de *contenido* y pegue el código CSS descargado en él.
4. Abra *App\_Start/bundle. config* y cambie *bootstrap. CSS* a *bootstrap-Theme. CSS*.

Vuelva a ejecutar el proyecto y la aplicación tendrá un nuevo aspecto. En la ilustración siguiente se muestra el efecto del tema Amelia:

![Tema Amelia de bootstrap](creating-web-projects-in-visual-studio/_static/image20.png)

Muchos temas de bootstrap están disponibles, tanto versiones gratuitas como Premium. Bootstrap también ofrece una amplia variedad de componentes de interfaz de usuario, como [listas](http://twitter.github.io/bootstrap/components.html#dropdowns)desplegables, [grupos de botones](http://twitter.github.io/bootstrap/components.html#buttonGroups)e [iconos](http://twitter.github.io/bootstrap/base-css.html#images). Para obtener más información acerca de bootstrap, consulte [el sitio de bootstrap](http://twitter.github.io/bootstrap/).

Si usa el diseñador de formularios Web Forms en Visual Studio, tenga en cuenta que el diseñador no es compatible con CSS3, por lo que no muestra con precisión todos los efectos de los temas de arranque o los cambios de diseño dinámicos. Sin embargo, las páginas de formularios Web Forms se mostrarán correctamente cuando se ven con un explorador.

<a id="add"></a>
## <a name="adding-support-for-additional-frameworks"></a>Agregar compatibilidad para marcos de trabajo adicionales

Al seleccionar una plantilla, se selecciona automáticamente la casilla de los marcos de trabajo usados por la plantilla. Por ejemplo, si selecciona la plantilla de **formularios Web Forms** , la casilla **formularios Web Forms** estará activada y no podrá desactivarla.

![Seleccione una plantilla:](creating-web-projects-in-visual-studio/_static/image21.png)

![Agregar marcos de trabajo](creating-web-projects-in-visual-studio/_static/image22.png)

Puede activar la casilla de un marco de trabajo que no está incluido en la plantilla para agregar compatibilidad para ese marco de trabajo cuando se crea el proyecto. Por ejemplo, para habilitar el uso de páginas de formularios Web Forms *. aspx* cuando haya seleccionado la plantilla MVC, active la casilla **formularios Web Forms** . O bien, para habilitar MVC cuando use la plantilla de formularios Web Forms, haga clic en la casilla **MVC** . Agregar un marco de trabajo permite la compatibilidad en tiempo de diseño y en tiempo de ejecución. Por ejemplo, si agrega compatibilidad con MVC a un proyecto de formularios Web Forms, podrá usar las vistas y los controladores de scaffolding.

Si combina formularios Web Forms y MVC en un proyecto y habilita [direcciones URL descriptivas](http://www.hanselman.com/blog/IntroducingASPNETFriendlyUrlsCleanerURLsEasierRoutingAndMobileViewsForASPNETWebForms.aspx) en formularios Web Forms, puede haber problemas de enrutamiento inesperados en los que una dirección URL tenga varios destinos posibles. Las rutas que se definen en primer lugar tendrán prioridad. Por ejemplo, si tiene un controlador de `Home` y una página *Home. aspx* , la dirección URL de `http://contoso.com/home` irá a *Home. aspx* si llama al método `EnableFriendlyUrls` antes de llamar al método de `MapRoute` en *RouteConfig.CS*, o la misma dirección URL irá a la vista predeterminada del controlador de `Home` si llama a `MapRoute` antes de `EnableFriendlyUrls`.

La adición de un marco no agrega ninguna funcionalidad de aplicación de ejemplo. Por ejemplo, si agrega compatibilidad con formularios Web Forms cuando ha seleccionado la plantilla MVC, no se crea ningún archivo de página principal *. aspx predeterminado* . Solo se agregan las carpetas, los archivos y las referencias necesarias para admitir el marco de trabajo. Por ese motivo, la adición de marcos no cambia las opciones de autenticación, que se implementan mediante código en las aplicaciones de ejemplo creadas por las plantillas. Por ejemplo, si selecciona la plantilla vacía y agrega compatibilidad con formularios Web Forms o MVC, el botón **configurar autenticación** seguirá deshabilitado.

En las secciones siguientes se describe brevemente el efecto de cada casilla.

### <a name="add-web-forms-support"></a>Compatibilidad con agregar formularios Web Forms

Crea carpetas vacías de *\_de aplicaciones* y de *modelos* y un archivo *global. asax* . Ya se han creado con todas las plantillas que no sean la plantilla vacía, por lo que si activa la casilla formularios Web Forms no se realiza ninguna diferencia en otras plantillas.

La plantilla de formularios Web Forms habilita las direcciones URL descriptivas de forma predeterminada, pero al agregar compatibilidad con formularios Web Forms a otras plantillas, al activar la casilla formularios Web Forms, las direcciones URL descriptivas no se habilitan automáticamente.

### <a name="add-mvc-support"></a>Agregar compatibilidad con MVC

Instala los paquetes NuGet MVC, Razor y Webpages, crea una *aplicación vacía\_datos*, *Controladores*, *modelos*y carpetas de *vistas* , crea una *aplicación\_* carpeta de inicio con el archivo *RouteConfig.CS* y crea un archivo *global. asax* .

### <a name="add-web-api-support"></a>Incorporación de compatibilidad con Web API

Instala los paquetes de NuGet WebApi y Newtonsoft. JSON, crea carpetas vacías de *\_de aplicaciones*, *Controladores*y *modelos* , crea una *aplicación\_* carpeta de inicio con el archivo *WebApiConfig.CS* y crea un archivo *global. asax* .

<a id="auth"></a>
## <a name="authentication-methods"></a>Métodos de autenticación

Visual Studio 2013 ofrece varias opciones de autenticación para las plantillas de Web Forms, MVC y Web API:

- [Sin autenticación](#noauth)
- [Cuentas de usuario individuales](#indauth) (ASP.net Identity, anteriormente conocido como pertenencia a ASP.net)
- [Cuentas de organización](#orgauth) (Windows Server Active Directory o Azure Active Directory)
- [Autenticación de Windows](#winauth) (Intranet)

![Cuadro de diálogo Configurar autenticación](creating-web-projects-in-visual-studio/_static/image23.png)

<a id="noauth"></a>

### <a name="no-authentication"></a>Sin autenticación

Si selecciona **sin autenticación**, la aplicación de ejemplo no contendrá ninguna página web para iniciar sesión, ninguna interfaz de usuario que indique quién ha iniciado sesión, ninguna clase de entidad para una base de datos de pertenencia y ninguna cadena de conexión para una base de datos de pertenencia.

<a id="indauth"></a>
### <a name="individual-user-accounts"></a>Cuentas de usuario individuales

Si selecciona **cuentas de usuario individuales**, la aplicación de ejemplo se configurará para usar ASP.net Identity (anteriormente conocido como pertenencia a ASP.net) para la autenticación de usuario. ASP.NET Identity permite a un usuario registrar una cuenta mediante la creación de un nombre de usuario y una contraseña en el sitio o mediante el iniciar sesión con proveedores de redes sociales, como Facebook, Google, cuenta de Microsoft o Twitter. El almacén de datos predeterminado para los perfiles de usuario en ASP.NET Identity es una SQL Server base de datos LocalDB, que se puede implementar en SQL Server o Azure SQL Database para el sitio de producción.

En Visual Studio 2013 estas características son las mismas que en Visual Studio 2012, pero se ha vuelto a escribir el código subyacente del sistema de pertenencia de ASP.NET. Entre las ventajas de la nueva base de código se incluyen las siguientes:

- El nuevo sistema de pertenencia se basa en [OWIN](http://owin.org/) en lugar de en el módulo de autenticación de formularios ASP.net. Esto significa que puede usar el mismo mecanismo de autenticación si usa formularios Web Forms o MVC en IIS, o si está autohospedado API o Signalr.
- La nueva base de datos de pertenencia se administra mediante Entity Framework Code First y todas las tablas se representan mediante clases de entidad que puede modificar. Esto significa que puede personalizar fácilmente el esquema de la base de datos y la interfaz de usuario web relacionada con el perfil para que se adapte a sus necesidades, y puede implementar fácilmente las actualizaciones mediante Migraciones de Code First.

El nuevo sistema de pertenencia se implementa automáticamente en las nuevas plantillas y se puede implementar manualmente en cualquier proyecto que tenga como destino .NET 4,5 o posterior.

ASP.NET Identity es una buena elección si va a crear un sitio web de Internet que es principalmente para clientes externos. Si su organización usa Active Directory u Office 365 y desea crear un proyecto que permita el inicio de sesión único para empleados y socios comerciales, la opción **cuentas organizativas** podría ser una opción mejor.

Para obtener más información acerca de la opción cuentas de usuario individuales, vea los siguientes recursos:

- [www.asp.net/Identity](../../../identity/index.md). Documentación sobre ASP.NET Identity en el sitio web de ASP.NET.
- [Cree una aplicación ASP.NET MVC 5 con el inicio de sesión de Facebook y Google OAuth2 y OpenID](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md). También muestra cómo personalizar los datos de Perfil de usuario.
- [API Web: servicios de autenticación externos](../../../web-api/overview/security/external-authentication-services.md)
- [Agregar inicios de sesión externos a la aplicación ASP.NET en Visual Studio 2013](https://blogs.msdn.com/b/webdev/archive/2013/06/27/adding-external-logins-to-your-asp-net-application-in-visual-studio-2013.aspx)

<a id="orgauth"></a>
### <a name="organizational-accounts"></a>Cuentas organizativas

Si selecciona cuentas de la **organización**, la aplicación de ejemplo se configurará para usar Windows Identity Foundation (WIF) para la autenticación basada en las cuentas de usuario de Azure Active Directory (Azure ad, que incluye Office 365) o Windows Server Active Directory. Para obtener más información, vea [Opciones de autenticación de cuenta de organización](#orgauthoptions) más adelante en este tema.

<a id="winauth"></a>
### <a name="windows-authentication"></a>Autenticación de Windows

Si selecciona **autenticación de Windows**, la aplicación de ejemplo se configurará para usar el módulo IIS de autenticación de Windows para la autenticación. La aplicación mostrará el dominio y el ID. de usuario de la cuenta de Active Directory o del equipo local que ha iniciado sesión en Windows, pero no incluirá el registro de usuario ni la interfaz de usuario de inicio de sesión. Esta opción está pensada para sitios web de la intranet.

También puede crear un sitio de intranet que use la autenticación de AD eligiendo la [opción local en cuentas](#orgauthonprem)de la organización. La opción local usa Windows Identity Foundation (WIF) en lugar del módulo de autenticación de Windows. Se requieren algunos pasos adicionales para configurar la opción local, pero WIF habilita las características que no están disponibles con el módulo de autenticación de Windows. Por ejemplo, con WIF puede configurar el acceso a las aplicaciones en Active Directory y consultar los datos de directorio.

<a id="orgauthoptions"></a>
## <a name="organizational-account-authentication-options"></a>Opciones de autenticación de cuenta de organización

El cuadro de diálogo **configurar autenticación** ofrece varias opciones para Azure Active Directory (Azure ad, que incluye Office 365) o la autenticación de cuentas de Active Directory de Windows Server (ad):

- [Organización de nube única](#orgauthsingle) (Azure ad o ad mediante la integración de directorios con Azure ad)
- [Nube: varias organizaciones](#orgauthmulti) (Azure ad o ad mediante la integración de directorios con Azure ad)
- [Local](#orgauthonprem) (ad)

Si desea probar una de las opciones de Azure AD pero aún no tiene una cuenta, [haga clic aquí para registrarse para obtener una cuenta de Azure ad](https://go.microsoft.com/fwlink/?LinkId=309942).

> [!NOTE]
> Si elige una de las opciones de Azure AD, el proyecto requiere una base de datos y debe iniciar sesión en una cuenta de administrador global para el inquilino de Azure AD. Escriba el nombre y la contraseña de una cuenta de organización (por ejemplo, admin@contoso.onmicrosoft.com) que tenga permisos administrativos para el inquilino de Azure AD.
> 
> **No escriba las credenciales de un cuenta de Microsoft (por ejemplo, contoso@hotmail.com) en el cuadro de diálogo de inicio de sesión.**

<a id="orgauthsingle"></a>
### <a name="cloud---single-organization-authentication"></a>Autenticación en la nube con una sola organización

![Autenticación de una sola organización](creating-web-projects-in-visual-studio/_static/image24.png)

Elija esta opción si desea habilitar la autenticación para las cuentas de usuario que se definen en un [inquilino](https://technet.microsoft.com/library/jj573650.aspx)de Azure ad. Por ejemplo, el sitio es contoso.com y estará disponible para los empleados de la empresa Contoso que se encuentran en el inquilino contoso.onmicrosoft.com. No podrá configurar Azure AD para permitir que los usuarios de otros inquilinos tengan acceso a la aplicación.

#### <a name="domain"></a>Dominio

Escriba el Azure AD dominio en el que desea configurar la aplicación, por ejemplo: `contoso.onmicrosoft.com`. Si tiene un [dominio personalizado](http://www.cloudidentity.com/blog/2013/04/14/adding-a-custom-domain-to-your-windows-azure-ad/), como `contoso.com` en lugar de `contoso.onmicrosoft.com`, puede escribirlo aquí.

#### <a name="access-level"></a>Nivel de acceso

Si la aplicación necesita consultar o actualizar la información de directorio mediante el Graph API, elija **Inicio de sesión único, leer datos de directorio** o **Inicio de sesión único, leer y escribir datos de directorio**. En caso contrario, elija **Inicio de sesión único**. Para obtener más información, consulte [niveles de acceso](https://msdn.microsoft.com/library/windowsazure/b08d91fa-6a64-4deb-92f4-f5857add9ed8#BKMK_AccessLevels) a las aplicaciones y [uso del Graph API para consultar Azure ad](https://msdn.microsoft.com/library/windowsazure/dn151791.aspx).

#### <a name="application-id-uri"></a>URI de Id. de aplicación

De forma predeterminada, la plantilla crea un URI de identificador de aplicación anexando el nombre del proyecto al dominio Azure AD. Por ejemplo, si el nombre del proyecto es `Example` y el dominio es `contoso.onmicrosoft.com`, el URI del ID. de aplicación se convierte en `https://contoso.onmicrosoft.com/Example`. Si desea especificar manualmente el URI del identificador de la aplicación, expanda la sección **más opciones** y escriba el URI del identificador de la aplicación en el cuadro de texto. El URI del ID. de aplicación debe comenzar con `https://`.

De forma predeterminada, si una aplicación que ya está aprovisionada en Azure AD tiene el mismo URI de ID. de aplicación que el que usa Visual Studio para el proyecto, el proyecto se conectará a la aplicación existente en lugar de aprovisionar uno nuevo. Si desea aprovisionar una nueva aplicación en ese caso, desactive la casilla **sobrescribir la entrada de la aplicación si ya existe una con el mismo identificador** .

Si la casilla de verificación **sobrescribir** está desactivada y Visual Studio encuentra una aplicación existente con el mismo URI de ID. de aplicación, se crea un nuevo URI anexando un número al URI que se va a usar. Por ejemplo, supongamos que el nombre del proyecto es `Example`, deja en blanco el cuadro de texto, desactiva la casilla de verificación **sobrescribir** y el inquilino de Azure ad ya tiene una aplicación con el URI `https://contoso.onmicrosoft.com/Example`. En ese caso, se aprovisionará una nueva aplicación con un URI de identificador de aplicación como `https://contoso.onmicrosoft.com/Example_20130619330903`.

#### <a name="provisioning-the-application-in-azure-ad"></a>Aprovisionamiento de la aplicación en Azure AD

Para aprovisionar la aplicación en Azure AD o conectar el proyecto a una aplicación existente, Visual Studio necesita las credenciales de un administrador global para el dominio. Al hacer clic en **Aceptar** en el cuadro de diálogo **configurar autenticación** , se le pedirá el nombre de usuario y la contraseña de un administrador global para el dominio especificado. Más adelante, al hacer clic en **crear proyecto** en el cuadro de diálogo **nuevo proyecto de ASP.net** , Visual Studio aprovisiona la aplicación en Azure ad. Tenga en cuenta que, como parte de este proceso, Visual Studio inserta valores de secreto de cliente en el archivo Web. config que expiran un año después de la creación.

Para obtener información sobre cómo crear aplicaciones que usan la autenticación **en la nube con una sola organización** , consulte los siguientes recursos:

- [Autenticación de Azure](../2012/windows-azure-authentication.md)
- [Incorporación del inicio de sesión a la aplicación web utilizando Azure AD](https://msdn.microsoft.com/library/windowsazure/dn151790.aspx)
- [Desarrollo de aplicaciones ASP.NET con Azure Active Directory](../../../identity/overview/getting-started/developing-aspnet-apps-with-windows-azure-active-directory.md)
- [Protección de ASP.NET Web API con Azure AD y componentes OWIN de Microsoft](https://msdn.microsoft.com/magazine/dn463788.aspx)

Los tutoriales todavía no se han actualizado para Visual Studio 2013; algunos de los tutoriales que le dirigen manualmente se automatizan en Visual Studio 2013.

<a id="orgauthmulti"></a>
### <a name="cloud---multi-organization-authentication"></a>Nube: autenticación de varias organizaciones

![Autenticación en varias organizaciones](creating-web-projects-in-visual-studio/_static/image25.png)

Elija esta opción si desea habilitar la autenticación para las cuentas de usuario definidas en varios [inquilinos](https://technet.microsoft.com/library/jj573650.aspx)de Azure ad. Por ejemplo, el sitio es contoso.com y estará disponible para los empleados de la empresa Contoso que se encuentran en el inquilino contoso.onmicrosoft.com y para los empleados de la compañía de Fabrikam que se encuentran en el inquilino fabrikam.onmicrosoft.com.

La configuración que especifique y el paso de aprovisionamiento de aplicaciones son similares a la [autenticación de una sola organización](#orgauthsingle).

Para obtener información sobre cómo crear aplicaciones que usen la autenticación **de la nube multiempresa** , consulte los siguientes recursos:

- [Integración sencilla de aplicaciones web con Azure Active Directory, ASP.NET &amp; Visual Studio](https://blogs.msdn.com/b/active_directory_team_blog/archive/2013/06/26/improved-windows-azure-active-directory-integration-with-asp-net-amp-visual-studio.aspx) en el blog del equipo de Active Directory.
- [Desarrollo de aplicaciones Web de varios inquilinos con Azure ad](https://msdn.microsoft.com/library/windowsazure/dn151789.aspx) tutorial. El tutorial todavía no se ha actualizado para Visual Studio 2013; parte de lo que el tutorial le indica realizar de forma manual se automatiza en Visual Studio 2013.
- [Tendrá que registrarse con su propia aplicación de ASP.net de varias organizaciones para poder iniciar sesión](http://www.cloudidentity.com/blog/2013/10/26/you-have-to-sign-up-with-your-own-multiple-organizations-asp-net-app-before-you-can-sign-in/). Blog de Vittorio Bertocci, en el que se explica cómo resolver un problema común que encuentran los usuarios al crear un proyecto que usa la autenticación de varias organizaciones.

<a id="orgauthonprem"></a>
### <a name="on-premises-organizational-authentication"></a>Autenticación de la organización local

![Autenticación de la organización local](creating-web-projects-in-visual-studio/_static/image26.png)

Elija esta opción si desea habilitar la autenticación para las cuentas de usuario definidas en Windows Server Active Directory (AD) y no desea usar Azure AD. Puede usar esta opción para crear un sitio de intranet o un sitio de Internet. Para un sitio de Internet, use Servicios de federación de Active Directory (AD FS) (ADFS) para proporcionar acceso a AD. Para obtener más información, consulte [uso de la opción de autenticación de la organización local (ADFS) con ASP.net en Visual Studio 2013](http://www.cloudidentity.com/blog/2014/02/12/use-the-on-premises-organizational-authentication-option-adfs-with-asp-net-in-visual-studio-2013/).

Para un sitio de intranet, como alternativa, puede elegir [autenticación de Windows](#winauth) en lugar de esta opción. En la opción de autenticación de Windows no es necesario proporcionar una dirección URL de documento de metadatos. Sin embargo, la autenticación de Windows no ofrece la posibilidad de controlar el acceso a las aplicaciones en Active Directory o para consultar datos de directorio.

#### <a name="on-premises-authority"></a>Autoridad local

Escriba una dirección URL que apunte al documento de metadatos. El documento de metadatos contiene las coordenadas de la autoridad. La aplicación usará esas coordenadas para controlar el flujo de inicio de sesión Web.

#### <a name="application-id-uri"></a>URI de Id. de aplicación

Proporcione un URI único que AD pueda usar para identificar esta aplicación o déjelo en blanco para permitir que Visual Studio cree una.

<a id="nextsteps"></a>
## <a name="next-steps"></a>Pasos siguientes

Este documento proporciona ayuda básica para crear un nuevo proyecto Web de ASP.NET en Visual Studio 2013. Para obtener más información sobre cómo usar para Visual Studio para el desarrollo web, vea [https://www.asp.net/visual-studio/](../../index.md).
