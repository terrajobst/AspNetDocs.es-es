---
uid: whitepapers/aspnet-web-deployment-content-map
title: 'Implementación web de ASP.NET: recursos recomendados | Microsoft Docs'
author: rick-anderson
description: En este tema se proporcionan vínculos a recursos de documentación sobre cómo implementar (publicar) aplicaciones Web ASP.NET en IIS mediante Visual Studio 2010, Visual Web de...
ms.author: riande
ms.date: 03/14/2014
ms.assetid: 58b583cd-c4ab-47a3-8527-8c92c298c91f
msc.legacyurl: /whitepapers/aspnet-web-deployment-content-map
msc.type: content
ms.openlocfilehash: 96873c8f2b0ad2415f371aceb651400c801a3338
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78517279"
---
# <a name="aspnet-web-deployment---recommended-resources"></a>Implementación web de ASP.NET - Recursos recomendados

> En este tema se proporcionan vínculos a recursos de documentación sobre cómo implementar (publicar) aplicaciones Web ASP.NET en IIS mediante Visual Studio 2010, Visual Web Developer 2010 y versiones posteriores.
> 
> Si conoce una gran entrada de blog, un subproceso de [stackoverflow](http://stackoverflow.com) o cualquier otro vínculo que le resulte útil, [envíenos un correo electrónico](mailto:aspnetue@microsoft.com?subject=Deployment%20Content%20Map) con el vínculo.
> 
> > [!NOTE] 
> > 
> > Muchos de estos recursos describen las características de implementación que solo están disponibles si instala una versión reciente de la [actualización de publicación Web de Visual Studio](https://go.microsoft.com/fwlink/?LinkID=208120). Algunas de las características solo están disponibles en Visual Studio 2012 o en Visual Studio 2013.

Este tema contiene las siguientes secciones:

- [Descripción de las opciones de implementación para proyectos Web](#understanding)
- [Búsqueda de proveedores de hospedaje para una aplicación de ASP.NET](#findinghosting)
- [Implementar una aplicación web desde Visual Studio](#fromvs)
- [Implementar una aplicación Web mediante la creación e instalación de un paquete de implementación web](#package)
- [Implementar una aplicación Web mediante un proceso de integración continua (CI)](#ci)
- [Usar transformaciones de Web. config para cambiar la configuración en el archivo Web. config de destino o el archivo app. config durante la implementación](#transforms)
- [Usar parámetros de Web Deploy para cambiar la configuración de la aplicación Web de destino durante la implementación](#webdeployparms)
- [Asegurarse de que una aplicación está sin conexión durante la implementación](#appoffline)
- [Implementar una base de datos o cambios en una base de datos como parte de la implementación de aplicaciones Web](#databasewithweb)
- [Implementar una base de datos de forma independiente de la implementación de aplicaciones Web](#databaseseparate)
- [Implementar una aplicación web que usa servicios de aplicaciones de ASP.NET, como la pertenencia y la generación de perfiles](#aspnetmembership)
- [Precompilación para la implementación](#precompiling)
- [Implementar una aplicación Web de la intranet](#intranet)
- [Automatizar tareas comunes de implementación que no están automatizadas de un solo uso](#automating)
- [Configuración de servidores web para que los programadores puedan implementar aplicaciones web en ellos mediante Web Deploy](#configuringservers)
- [Configuración de servidores para un proveedor de hospedaje](#hostingprovider)
- [Solución de problemas de implementación](#troubleshooting)
- [Obtener ayuda con una pregunta de implementación específica](#gettinghelp)
- [Recursos adicionales](#additional)

<a id="understanding"></a>

## <a name="understanding-deployment-options-for-web-projects"></a>Descripción de las opciones de implementación para proyectos Web

- [Información general sobre la implementación web para Visual Studio y ASP.net](https://msdn.microsoft.com/library/dd394698.aspx) (MSDN).
- [Cómo implementar un sitio web de Windows Azure](https://docs.microsoft.com/azure/app-service-web/web-sites-deploy). Explica las opciones y vínculos a los recursos para implementar proyectos web en sitios web de Windows Azure, incluida la [entrega continua](../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/continuous-integration-and-continuous-delivery.md) (automatizada desde el [control de código fuente](../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control.md)), así como el uso de Visual Studio.
- [Mejoras en la publicación Web de Visual Studio 2012](../visual-studio/overview/2012/visual-studio-2012-web-publishing-improvements.md) (vídeo de Scott Hanselman).
- [Introducción a la implementación web en VS 2010](http://vishaljoshi.blogspot.com/2009/09/overview-post-for-web-deployment-in-vs.html) (blog de Vishal Joshi). Una entrada de blog anterior, pero algunos de los recursos de Visual Studio 2010 a los que está vinculada tienen información que sigue siendo pertinente para Visual Studio 2012.

<a id="findinghosting"></a>

## <a name="finding-hosting-providers-for-an-aspnet-application"></a>Búsqueda de proveedores de hospedaje para una aplicación de ASP.NET

- [Hospedaje de ASP.NET](https://asp.net/hosting)

<a id="fromvs"></a>

## <a name="deploying-a-web-application-from-visual-studio"></a>Implementar una aplicación web desde Visual Studio

- [Cómo implementar un sitio web de Windows Azure](https://docs.microsoft.com/azure/app-service-web/web-sites-deploy). Explica las opciones de y proporciona vínculos a recursos para implementar proyectos web en sitios web de Windows Azure. Incluye una sección sobre la implementación desde Visual Studio.
- [Implementación web de ASP.NET con Visual Studio](../web-forms/overview/deployment/visual-studio-web-deployment/introduction.md). la serie de tutoriales de 12 partes muestra cómo implementar aplicaciones web con bases de datos de SQL Server. En la implementación de base de datos usa el proveedor dbDacFx y Migraciones de Entity Framework Code First. También incluye información sobre las [transformaciones del archivo Web. config](../web-forms/overview/deployment/visual-studio-web-deployment/web-config-transformations.md), la implementación de [archivos individuales](../web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-code-update.md#specificfiles), la [implementación desde la línea de comandos](../web-forms/overview/deployment/visual-studio-web-deployment/command-line-deployment.md)y [Cómo personalizar la canalización de publicación Web de Visual Studio mediante la edición de archivos. pubxml](../web-forms/overview/deployment/visual-studio-web-deployment/deploying-extra-files.md). Se aplica a todos los proyectos Web de ASP.NET, incluidos los formularios Web Forms, MVC y Web API).
- [Cómo: implementar un proyecto web mediante publicación con un solo clic en Visual Studio](https://msdn.microsoft.com/library/dd465337.aspx) (información de referencia para el Asistente para publicación Web de Visual Studio)
- [Implementación de una aplicación Web de ASP.net con SQL Server Compact con Visual Studio](../web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-introduction-1-of-12.md). Se trata de una versión anterior de **ASP.net web Deployment con Visual Studio** que se muestra en la parte superior de esta sección. Principalmente útil ahora para obtener información acerca de cómo implementar SQL Server Compact bases de datos y cómo migrar de SQL Server Compact a una edición completa de SQL Server.
- [Aplicación de niveles múltiples .net que usa tablas, colas y blobs de almacenamiento](https://code.msdn.microsoft.com/Windows-Azure-Multi-Tier-eadceb36) (sitio Microsoft Azure). serie de tutoriales de 5 partes, se muestra cómo crear un proyecto de MVC e implementarlo en un servicio en la nube de Windows Azure.

<a id="package"></a>
## <a name="deploying-a-web-application-by-creating-and-installing-a-web-deployment-package"></a>Implementar una aplicación Web mediante la creación e instalación de un paquete de implementación web

- [Cómo: crear un paquete de implementación web en Visual Studio](https://msdn.microsoft.com/library/dd465323.aspx) (MSDN).
- [Cómo: instalar un paquete de implementación mediante el archivo deploy. cmd creado por Visual Studio](https://msdn.microsoft.com/library/ff356104.aspx) (MSDN).
- [Uso de un paquete de web deploy para implementar en IIS en el cuadro dev y en un host de terceros](http://sedodream.com/2011/11/08/UsingAWebDeployPackageToDeployToIISOnTheDevBoxAndToAThirdPartyHost.aspx) (blog de Sayed Hashimi). Cómo usar el administrador de IIS para instalar un paquete de implementación en IIS en el equipo local y en una empresa de hospedaje que admita el administrador de IIS para la administración remota.
- [Compilar un paquete de web deploy desde Visual Studio 2010](https://www.iis.net/learn/publish/using-web-deploy/building-a-web-deploy-package-from-visual-studio-2010) (sitio Web de IIS.net). Incluye instrucciones para la creación e instalación de paquetes de la línea de comandos.
- [Paquete una vez publicado en cualquier lugar](http://sedodream.com/2012/03/14/PackageWebUpdatedAndVideoBelow.aspx) (blog de Sayed Hashimi). Presenta un paquete de NuGet que automatiza el proceso de transformación del archivo Web. config para varios entornos de destino, de modo que puede implementar un paquete en varios servidores. Vea también el [vídeo PackageWeb](https://www.youtube.com/watch?v=-LvUJFI8CzM) de Sayed Hashimi.

Vea también la sección siguiente.

<a id="ci"></a>

## <a name="deploying-a-web-application-using-a-continuous-integration-ci-process"></a>Implementar una aplicación Web mediante un proceso de integración continua (CI)

- [Integración continua y entrega continua (creación de aplicaciones en la nube reales con Windows Azure).](../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/continuous-integration-and-continuous-delivery.md) Capítulo del libro electrónico en el que se presenta la integración continua y la entrega continua.
- [Cómo implementar un sitio web de Windows Azure](https://docs.microsoft.com/azure/app-service-web/web-sites-deploy). Explica las opciones y vínculos a los recursos para implementar proyectos web en sitios web de Windows Azure. Incluye una sección sobre cómo automatizar la implementación desde el control de código fuente.
- [Implementación de aplicaciones web en escenarios empresariales](../web-forms/overview/deployment/deploying-web-applications-in-enterprise-scenarios/deploying-web-applications-in-enterprise-scenarios.md). en la serie de tutoriales de 40, se muestra cómo automatizar la implementación en un proceso de CI mediante Visual Studio 2010 y Team Foundation Server 2010.
- [Dentro del Microsoft Build Engine: usar MSBuild y Team Foundation Build, por Sayed Hashimi y William Bartholomew](http://msbuildbook.com). Se trata de un libro, no un recurso Web, pero es una guía esencial para aprender a configurar MSBuild para escenarios de integración continua.
- [Paquete de extensión de MSBuild](https://github.com/mikefourie/MSBuildExtensionPack). Incluye tareas de implementación.
- [Guía de personalización de Team Foundation Build](https://aka.ms/vsarsolutions). La documentación de los rangos de ALM en la configuración de Team Foundation Server cubre la implementación web e incluye tutoriales y vídeos.
- [SlowCheetah transformaciones XML de un servidor CI](http://sedodream.com/2011/12/12/SlowCheetahXMLTransformsFromACIServer.aspx) (blog de Sayed Hashimi). Explica cómo usar SlowCheetah, un complemento de Visual Studio para transformar app. config y otros archivos XML.

Vea también [asegurarse de que una aplicación está sin conexión durante la implementación](aspnet-web-deployment-content-map.md#appoffline) más adelante en esta página.

<a id="transforms"></a>

## <a name="using-webconfig-transformations-to-change-settings-in-the-destination-webconfig-file-or-appconfig-file-during-deployment"></a>Usar transformaciones de Web. config para cambiar la configuración en el archivo Web. config de destino o el archivo app. config durante la implementación

- [Transformaciones del archivo Web. config](../web-forms/overview/deployment/visual-studio-web-deployment/web-config-transformations.md).
- [Sintaxis de transformación de Web. config para la implementación de proyectos web con Visual Studio](https://msdn.microsoft.com/library/dd465326.aspx) (MSDN).
- [Herramientas web 2012,2-transformaciones de Web. config](https://www.youtube.com/watch?v=HdPK8mxpKEI) (vídeo de YouTube por Sayed Hashimi). Muestra cómo configurar y obtener una vista previa de las transformaciones de Web. config.
- [Cómo deshabilitar la transformación de Web. config?](https://msdn.microsoft.com/library/ee942158.aspx#disable_web_config_transformation) (MSDN).
- [¿Cuándo debo usar Web Deploy parámetros en lugar de transformaciones de Web. config?](https://msdn.microsoft.com/library/ee942158.aspx#web_deploy_parameters) (MSDN).
- [XDT (transformación de documento XML) publicado en Codeplex.com](https://blogs.msdn.com/b/webdev/archive/2013/04/23/xdt-xml-document-transform-released-on-codeplex-com.aspx) (blog de herramientas y desarrollo web de .net). Anuncia la disponibilidad del código fuente para el motor de transformación de archivos Web. config y enumera algunas herramientas que lo usan.
- [Sitios web de Windows Azure: Cómo funcionan las cadenas de aplicación y las cadenas de conexión](https://blogs.msdn.com/b/windowsazure/archive/2013/07/17/windows-azure-web-sites-how-application-strings-and-connection-strings-work.aspx) (blog de Microsoft Azure). Una alternativa a las transformaciones de Web. config si el entorno de destino son sitios web de Windows Azure y desea transformar `appSettings` o `connectionStrings`.

<a id="webdeployparms"></a>

## <a name="using-web-deploy-parameters-to-change-settings-in-the-destination-web-application-during-deployment"></a>Usar parámetros de Web Deploy para cambiar la configuración de la aplicación Web de destino durante la implementación

- [Cómo: usar parámetros de web deploy en un paquete de implementación web](https://msdn.microsoft.com/library/ff398068.aspx) (MSDN).
- [MSDeploy: Cómo actualizar la configuración de la aplicación en la publicación según el perfil de publicación](http://sedodream.com/2013/03/02/MSDeployHowToUpdateAppSettingsOnPublishBasedOnThePublishProfile.aspx) (blog de Sayed Hashimi). Muestra cómo integrar los parámetros de web deploy en perfiles de publicación de Visual Studio.
- [Parametrización web deploy](https://www.iis.net/learn/publish/using-web-deploy/web-deploy-parameterization) (sitio Web de IIS.net).
- [Web deploy parametrización en acción](http://vishaljoshi.blogspot.com/2010/07/web-deploy-parameterization-in-action.html) (blog de Vishal Joshi).
- [Web deploy la transformación de la parametrización y la Web. config](http://vishaljoshi.blogspot.com/2010/06/parameterization-vs-webconfig.html) (blog de Vishal Joshi).
- [Sitios web de Windows Azure: Cómo funcionan las cadenas de aplicación y las cadenas de conexión](https://blogs.msdn.com/b/windowsazure/archive/2013/07/17/windows-azure-web-sites-how-application-strings-and-connection-strings-work.aspx) (blog de Microsoft Azure). Una alternativa a los parámetros de web deploy si el entorno de destino son sitios web de Windows Azure y desea parametrizar `appSettings` o `connectionStrings`.

<a id="appoffline"></a>

## <a name="making-sure-an-application-is-off-line-during-deployment"></a>Asegurarse de que una aplicación está sin conexión durante la implementación

- [Implementación web de ASP.net con Visual Studio: implementación de una actualización de código](../web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-code-update.md). Vea la sección **poner la aplicación sin conexión durante la implementación.**
- Desconectar [una aplicación antes de publicar](https://www.iis.net/learn/publish/deploying-application-packages/taking-an-application-offline-before-publishing) (sitio de IIS.net). Explica una característica integrada en Web Deploy 3,0 que automatiza el control de una aplicación\_archivo. htm sin conexión. Esta característica no funciona con archivos. htm sin conexión\_aplicación personalizada.
- [Cómo poner la aplicación web sin conexión durante la publicación](http://sedodream.com/2012/01/08/HowToTakeYourWebAppOfflineDuringPublishing.aspx) (blog de Sayed Hashimi). Cómo automatizar el proceso de uso de una aplicación personalizada\_archivo. htm sin conexión.
- [Actualizaciones de publicación web para aplicaciones sin conexión y usechecksum](https://blogs.msdn.com/b/webdev/archive/2013/10/30/web-publishing-updates-for-app-offline-and-usechecksum.aspx) (blog de desarrollo web de Microsoft). Otra opción para automatizar el uso de la aplicación\_archivo. htm sin conexión.
- [Web Deploy 3,5 RTW](https://blogs.iis.net/msdeploy/archive/2013/07/09/webdeploy-3-5-rtw.aspx) (sitio de IIS.net). Nueva característica de Web Deploy 3,5 para la aplicación personalizada\_archivos. htm sin conexión.

<a id="databasewithweb"></a>

## <a name="deploying-a-database-or-changes-to-a-database-as-part-of-web-application-deployment"></a>Implementar una base de datos o cambios en una base de datos como parte de la implementación de aplicaciones Web

- [Configuración de la implementación de bases de datos en Visual Studio](https://msdn.microsoft.com/library/dd394698.aspx#dbdeployment) (MSDN). Información general de las opciones para implementar una base de datos con un proyecto Web.
- [Implementación web de ASP.NET con Visual Studio](../web-forms/overview/deployment/visual-studio-web-deployment/introduction.md). serie de tutoriales de 12 partes, muestra la implementación de bases de datos mediante el proveedor dbDacFx y Migraciones de Entity Framework Code First.
- [Cómo: implementar un proyecto web mediante publicación con un solo clic en Visual Studio](https://msdn.microsoft.com/library/dd465337.aspx) (MSDN).
- [Implemente una aplicación ASP.NET MVC 5 segura con suscripción, OAuth y SQL Database en un sitio web de Windows Azure](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data). Un tutorial largo que compila e implementa una aplicación que usa una sola base de datos de SQL Server para los datos de la pertenencia y de las aplicaciones.
- [Implementación de una aplicación Web de ASP.net con SQL Server Compact con Visual Studio](../web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-introduction-1-of-12.md). la serie de tutoriales de 12 partes muestra cómo implementar SQL Server Compact bases de datos y cómo migrar de SQL Server Compact a una edición completa de SQL Server.

Vea también implementar una aplicación web creando e instalando un paquete de implementación web e implementando una aplicación Web mediante un proceso de integración continua (CI) anteriormente en esta página.

<a id="databaseseparate"></a>

## <a name="deploying-a-database-separately-from-web-application-deployment"></a>Implementar una base de datos de forma independiente de la implementación de aplicaciones Web

- [SQL Server Data Tools](https://msdn.microsoft.com/library/hh272686(v=vs.103).aspx) (MSDN).
- [Incluir datos en un proyecto de base de datos de SQL Server](https://blogs.msdn.com/b/ssdt/archive/2012/02/02/including-data-in-an-sql-server-database-project.aspx) (blog del equipo de SQL Server Data Tools). Cómo implementar el esquema y los datos al implementar una base de datos.
- [Cómo implementar una base de datos en Windows Azure](https://docs.microsoft.com/azure/sql-database/sql-database-cloud-migrate) (sitio Microsoft Azure)
- [Migrar bases de datos a Windows Azure SQL Database (anteriormente SQL Azure)](https://msdn.microsoft.com/library/windowsazure/ee730904.aspx) (MSDN).
- [Migración de una base de datos a SQL Azure mediante SSDT](https://blogs.msdn.com/b/ssdt/archive/2012/04/19/migrating-a-database-to-sql-azure-using-ssdt.aspx) (blog del equipo de SQL Server Data Tools).
- [Migrar aplicaciones centradas en datos a Windows Azure](https://msdn.microsoft.com/library/jj156154.aspx) (MSDN).
- [Migrar bases de datos de SQL Server a Windows Azure SQL Database](https://msdn.microsoft.com/library/windowsazure/jj156160.aspx) (MSDN).

<a id="aspnetmembership"></a>

## <a name="deploying-a-web-application-that-uses-aspnet-application-services-such-as-membership-and-profiling"></a>Implementar una aplicación web que usa servicios de aplicaciones de ASP.NET, como la pertenencia y la generación de perfiles

- [Implemente una aplicación ASP.NET MVC 5 segura con suscripción, OAuth y SQL Database en un sitio web de Windows Azure](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data). Un tutorial largo que compila e implementa una aplicación que usa una sola base de datos de SQL Server para los datos de la pertenencia y de las aplicaciones.
- [ASP.net Identity](https://asp.net/identity/). Recursos para ASP.NET Identity.
- [Implementación web de ASP.NET con Visual Studio](../web-forms/overview/deployment/visual-studio-web-deployment/introduction.md). en la serie de tutoriales de 12 partes, se muestra cómo implementar una base de datos de pertenencia a ASP.NET.
- [Configuración de un sitio web que usa servicios de aplicación](../web-forms/overview/older-versions-getting-started/deploying-web-site-projects/configuring-a-website-that-uses-application-services-cs.md). En los proyectos de sitio web, pero también es pertinente para los proyectos de aplicación Web.
- [Usuarios y roles en el sitio web de producción](../web-forms/overview/older-versions-getting-started/deploying-web-site-projects/users-and-roles-on-the-production-website-cs.md). En los proyectos de sitio web, pero también es pertinente para los proyectos de aplicación Web.

<a id="precompiling"></a>

## <a name="precompiling-for-deployment"></a>Precompilación para la implementación

- [Información general sobre la precompilación de proyectos de aplicación Web de ASP.net](https://msdn.microsoft.com/library/aa983464.aspx) (MSDN).
- [Pestaña empaquetar/publicar web, propiedades del proyecto](https://msdn.microsoft.com/library/dd410108.aspx) (MSDN).
- [Cuadro de diálogo Configuración de precompilación avanzada](https://msdn.microsoft.com/library/hh475319.aspx) (MSDN).

<a id="intranet"></a>

## <a name="deploying-an-intranet-web-application"></a>Implementar una aplicación Web de la intranet

- [Use la opción de autenticación de la organización local (ADFS) con ASP.net en Visual Studio 2013](http://www.cloudidentity.com/blog/2014/02/12/use-the-on-premises-organizational-authentication-option-adfs-with-asp-net-in-visual-studio-2013/) (blog de Vittorio BERTOCCI).
- [Cómo crear un sitio de intranet mediante ASP.NET MVC](https://msdn.microsoft.com/library/gg703322(VS.98).aspx) (MSDN). El tutorial anterior writen para Visual Studio 2010 no refleja los cambios principales en las plantillas de proyecto de la intranet introducidas en Visual Studio 2013.

<a id="automating"></a>

## <a name="automating-common-deployment-tasks-that-are-not-automated-out-of-the-box"></a>Automatizar tareas comunes de implementación que no están automatizadas de un solo uso

- [Implementación web de ASP.net con Visual Studio: implementación de archivos adicionales](../web-forms/overview/deployment/visual-studio-web-deployment/deploying-extra-files.md).
- [Configuración de permisos de carpeta en Web Publish](http://sedodream.com/2011/11/08/SettingFolderPermissionsOnWebPublish.aspx) (blog de Sayed Hashimi).
- [Cómo extender el archivo de destinos para incluir la configuración del registro para un paquete de proyecto web](https://blogs.msdn.com/webdevtools/archive/2010/02/09/how-to-extend-target-file-to-include-registry-settings-for-web-project-package.aspx) (blog de herramientas de desarrollo web).
- [Extender la transformación XML (Web. config)](http://sedodream.com/2010/09/09/ExtendingXMLWebconfigConfigTransformation.aspx) (blog de Sayed Hashimi). Muestra cómo crear transformaciones de XDT personalizadas.
- El [proveedor personalizado de la herramienta de implementación web (MSDeploy) toma 1](http://sedodream.com/2010/03/11/WebDeploymentToolMSDeployCustomProviderTake1.aspx) (blog de Sayed Hashimi). Muestra cómo crear un proveedor personalizado de Web Deploy.
- [Cómo empaquetar e implementar componentes com](https://blogs.msdn.com/webdevtools/archive/2010/03/03/how-to-package-and-deploy-com-component.aspx) (blog de herramientas de desarrollo web).
- [Cómo empaquetar ensamblados .net](https://blogs.msdn.com/webdevtools/archive/2010/02/19/how-to-package-com-component.aspx) (blog de herramientas de desarrollo web). Cómo implementar ensamblados en la GAC.
- [Incluir todo: inicializar la máquina virtual de Windows Azure para el servidor Web con IIS, Web deploy y otros tipos](http://www.tugberkugurlu.com/archive/script-out-everything-initialize-your-windows-azure-vm-for-your-web-server-with-iis-web-deploy-and-other-stuff) (blog de Tugberk Ugurlu).

<a id="configuringservers"></a>

## <a name="configuring-web-servers-so-that-developers-can-deploy-web-applications-to-them-using-web-deploy"></a>Configuración de servidores web para que los programadores puedan implementar aplicaciones web en ellos mediante Web Deploy

- [Instalación y configuración de web deploy para implementaciones de administrador y de no administrador](https://www.iis.net/learn/install/installing-publishing-technologies/installing-and-configuring-web-deploy) (sitio de IIS.net).

<a id="hostingprovider"></a>

## <a name="configuring-servers-for-a-hosting-provider"></a>Configuración de servidores para un proveedor de hospedaje

- [Guía de implementación de hospedaje de Microsoft ASP.net 4](https://go.microsoft.com/fwlink/?LinkId=191365) (centro de descarga de Microsoft).
- [Generar un archivo XML de perfil](https://www.iis.net/learn/web-hosting/joining-the-web-hosting-gallery/generate-a-profile-xml-file) (sitio de IIS.net).

<a id="troubleshooting"></a>

## <a name="troubleshooting-deployment-problems"></a>Solución de problemas de implementación

- [Solucionar problemas de sitios web de Windows Azure en Visual Studio](https://docs.microsoft.com/azure/app-service-web/web-sites-dotnet-troubleshoot-visual-studio) (sitio Microsoft Azure).
- [Implementación web de ASP.net con Visual Studio: solución de problemas](../web-forms/overview/deployment/visual-studio-web-deployment/troubleshooting.md).
- [Solución de problemas comunes con Web deploy](https://www.iis.net/learn/publish/troubleshooting-web-deploy/troubleshooting-common-problems-with-web-deploy).
- [Web deploy códigos de error](https://www.iis.net/learn/publish/troubleshooting-web-deploy/web-deploy-error-codes) (sitio IIS.net).
- [Preguntas más frecuentes sobre la implementación web para Visual Studio y ASP.net](https://msdn.microsoft.com/library/ee942158.aspx) (MSDN).
- [Diferencias principales entre IIS y el servidor de desarrollo de ASP.net](../web-forms/overview/older-versions-getting-started/deploying-web-site-projects/core-differences-between-iis-and-the-asp-net-development-server-cs.md).
- [Diferencias de configuración comunes entre el desarrollo y la producción](../web-forms/overview/older-versions-getting-started/deploying-web-site-projects/common-configuration-differences-between-development-and-production-cs.md).
- [Hospedaje de aplicaciones de ASP.net en confianza media](http://www.4guysfromrolla.com/articles/100307-1.aspx) (4 chicos del sitio Rolla).

<a id="gettinghelp"></a>

## <a name="getting-help-with-a-specific-deployment-question"></a>Obtener ayuda con una pregunta de implementación específica

- [Foro de configuración e implementación de ASP.net](https://forums.asp.net/26.aspx/1?Configuration and Deployment).
- [StackOverflow.com](http://www.StackOverflow.com).

<a id="additional"></a>

## <a name="additional-resources"></a>Recursos adicionales

En esta sección se proporcionan vínculos a recursos adicionales que son útiles para obtener más información sobre cómo usar las herramientas de implementación de Visual Studio e IIS.

Los siguientes blogs suelen contener información sobre la implementación web de Visual Studio:

- [Herramientas de desarrollo web en el blog de Microsoft](https://blogs.msdn.com/b/webdevtools/).
- [Blog de Sayed Hashimi](http://www.sedodream.com/).

Los recursos siguientes proporcionan documentación sobre Web Deploy, el marco de trabajo de IIS que Visual Studio usa para realizar tareas de implementación de proyectos de aplicación Web. Puede formular preguntas sobre Web Deploy en el [Foro de la herramienta de implementación web](https://go.microsoft.com/fwlink/?LinkId=149411) en el sitio web de IIS.net.

- [Introducción a la web deploy](https://www.iis.net/learn/publish/using-web-deploy/introduction-to-web-deploy).
- [Instalación y configuración de web deploy](https://www.iis.net/learn/install/installing-publishing-technologies/installing-and-configuring-web-deploy).
- [Scripts de PowerShell para automatizar la instalación de web deploy](https://www.iis.net/learn/publish/using-web-deploy/powershell-scripts-for-automating-web-deploy-setup).
- [Herramienta de implementación web](https://go.microsoft.com/fwlink/?LinkId=151481). Nodo de tabla de contenido de nivel superior para la documentación de Web Deploy en el sitio de TechNet. Incluye información de referencia útil, pero la mayoría de las páginas de TechNet no se han actualizado durante años.
- [Espacio de nombres Microsoft. Web. Deployment](https://go.microsoft.com/fwlink/?LinkId=148630). La documentación de la API no se ha actualizado desde la versión 1,0.
- [Blog del equipo de implementación web de Microsoft](https://blogs.iis.net/msdeploy/default.aspx).
- [Pestaña publicar en el sitio web de IIS.net](https://www.iis.net/learn/publish).
