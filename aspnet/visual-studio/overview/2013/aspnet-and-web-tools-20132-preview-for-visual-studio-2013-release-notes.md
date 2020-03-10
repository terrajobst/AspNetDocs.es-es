---
uid: visual-studio/overview/2013/aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes
title: ASP.NET and Web Tools 2013,2 para las notas de la versión de Visual Studio 2013 | Microsoft Docs
author: microsoft
description: ''
ms.author: riande
ms.date: 03/06/2014
ms.assetid: 7ef5f73c-ca60-43c1-bdb2-702800347e7e
msc.legacyurl: /visual-studio/overview/2013/aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes
msc.type: authoredcontent
ms.openlocfilehash: 22d4d4afd6963f23d6cfef1745a859c20b69d599
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78505453"
---
# <a name="aspnet-and-web-tools-20132--for-visual-studio-2013-release-notes"></a>Notas de la versión de ASP.NET and Web Tools 2013.2 para Visual Studio 2013

por [Microsoft](https://github.com/microsoft)

## <a name="installation-notes"></a>Notas de la instalación

ASP.NET and Web Tools para Visual Studio 2013,2 se incluyen en el instalador principal y se pueden descargar como parte de [Visual Studio 2013 Update 2](https://go.microsoft.com/fwlink/?LinkId=390521).

## <a name="documentation"></a>Documentación

Los tutoriales y otra información sobre ASP.NET and Web Tools para Visual Studio 2013,2 están disponibles en el [sitio web de ASP.net](https://www.asp.net/).

## <a name="software-requirements"></a>Requisitos de software

ASP.NET and Web Tools para Visual Studio 2013,2 requiere Visual Studio 2013.

## <a name="new-features-in-aspnet-and-web-tools-for-visual-studio-20132"></a>Nuevas características de ASP.NET and Web Tools para Visual Studio 2013,2

En las secciones siguientes se describen las características que se han introducido en la versión.

- [Una plantilla de proyecto de ASP.NET](#oneaspnet)
- [Compatibilidad con SSL al iniciar aplicaciones web en IIS Express](#ssl)
- [Mejoras del editor web de Visual Studio](#vswebeditor)
- [Vínculo con exploradores](#browserlink)
- [Compatibilidad con Web Apps de Azure App Service en Visual Studio](#waws)
- [Crear recursos remotos de Azure al crear un nuevo proyecto web](#AzureResources)
- [Mejoras en la publicación en Web](#webpublish)
- [Scaffolding de ASP.NET](#scaffolding)
- [NuGet 2.8.1](#nuget)
- [Formularios Web Forms ASP.NET](#webforms)
- [ASP.NET MVC 5.1.2](#mvc)
- [ASP.NET Web API 2.1.2](#webapi)
- [ASP.NET Web Pages 3.1.2](#webpages)
- [Entity Framework 6,1](#ef)
- [ASP.NET Identity 2.0.0](#identity)
- [Componentes OWIN de Microsoft](#owin)
- [ASP.NET Signalr 2.0.2](#signalr)

<a id="oneaspnet"></a>
### <a name="one-aspnet-project-templates"></a>Una plantilla de proyecto de ASP.NET

- Actualizaciones de plantillas de proyecto de ASP.NET para admitir la confirmación de la cuenta y el restablecimiento de contraseña.
- Actualice ASP.NET Web API plantilla para admitir la autenticación mediante cuentas organizativas locales.
- La plantilla ASP.NET SPA ahora contiene autenticación basada en MVC y vistas del lado servidor. La plantilla tiene un controlador WebAPI al que solo pueden acceder los usuarios autenticados.

<a id="ssl"></a>
### <a name="support-ssl-when-launching-web-applications-on-iis-express"></a>Compatibilidad con SSL al iniciar aplicaciones web en IIS Express

Para eliminar la advertencia de seguridad al examinar y depurar HTTPS en localhost, se ha agregado un cuadro de diálogo para permitir que Internet Explorer y Chrome confíen en el certificado SSL Express autofirmado de IIS.

Por ejemplo, se puede establecer una propiedad de proyecto web para utilizar SSL. Haga clic en F4 para abrir el cuadro de diálogo Propiedades. Cambie **SSL habilitado** a true. Copie la URL de SSL.

![Propiedad SSL habilitado](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image1.png)

Establezca la pestaña Web de la página de propiedades del proyecto web para usar la dirección URL basada en HTTPS (la dirección URL de SSL será `https://localhost:44300/` a menos que haya creado previamente sitios Web SSL).

![Establecer la dirección URL del proyecto (HTTPS)](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image2.png)

Presione CTRL+F5 para ejecutar la aplicación. Siga las instrucciones para confiar en el certificado autofirmado que ha generado IIS Express.

![ADVERTENCIA de SSL](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image3.png)

Lea el cuadro de diálogo **Advertencia de seguridad** y, a continuación, haga clic en **sí** si desea instalar el certificado que representa localhost.

![Advertencia de seguridad](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image4.png)

El sitio se mostrará en IE o Chrome sin la advertencia de certificado en el explorador.

![Página HTTPS sin advertencias](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image5.png)

Firefox usa su propio almacén de certificados, por lo que mostrará una advertencia.

<a id="vswebeditor"></a>
### <a name="visual-studio-web-editor-enhancements"></a>Mejoras del editor web de Visual Studio

- **Nuevo elemento de proyecto JSON y editor**: se ha agregado un elemento de proyecto JSON y un editor a Visual Studio. Las características actuales del editor JSON incluyen coloración, validación de sintaxis, finalización de llaves, esquematización, configuración de opciones de herramientas y mucho más.

    ![Editor JSON](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image6.png)

    IntelliSense ahora admite el [esquema JSON](http://json-schema.org/) V3 y V4. Hay un cuadro combinado de esquema para elegir los esquemas existentes, editar la ruta de acceso del esquema local o simplemente arrastrar el archivo JSON del proyecto para obtener la ruta de acceso relativa.

    ![IntelliSense para JSON](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image7.png)    ![Editor de esquemas JSON](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image8.png)
- **Nuevo Sass (SCSS) editor**: hemos agregado menos en VS2013 RTM y ahora tenemos un elemento de proyecto de Sass y un editor. Las características del editor de Sass son comparables con el editor menos e incluyen coloración, variables y mixins IntelliSense, comentario/uncomment, información rápida, formato, validación de sintaxis, esquematización, definición de Goto, selector de colores, configuración de opciones de herramientas, etc.

    ![Agregar nuevo elemento: hoja de estilos SCSS](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image9.png)    ![Editor de hoja de estilos](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image10.png)
- **Nuevo selector de URL en documentos HTML, Razor, CSS, menos y Sass:** VS 2013 se incluye sin selector de URL fuera de las páginas de formularios Web Forms. El nuevo selector de URL para los editores HTML, Razor, CSS, LESS y Sass es un selector de escritura fluida sin cuadro de diálogo que entiende '.. ' y filtra las listas de archivos adecuadamente para etiquetas y vínculos IMG.

    ![Selector de URL para etiqueta de imagen](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image11.png)    ![Selector de URL para vistas](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image12.png)    ![Selector de URL para CSS](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image13.png)
- **Actualizaciones a menos editor agregando más características**
- **Actualización del Knockout de IntelliSense**: hemos agregado una sintaxis de cobertura no estándar para IntelliSense, la sintaxis "KO-vs-editor viewModel:". Se puede utilizar para enlazar varios modelos de vista en una página mediante comentarios en el formulario:

    ![IntelliSense de cobertura](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image14.png)

    También hemos agregado compatibilidad con IntelliSense anidado de ViewModel, por lo que puede profundizar en objetos profundamente anidados en el ViewModel.

    `<div data-bind="text: foo.bar.baz.etc" />`

    IntelliSense mostrado es el IntelliSense completo del objeto JavaScript.

    ![IntelliSense que muestra el objeto JavaScript completo](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image15.png)
- **Nuevo selector de URL en documentos HTML, Razor, CSS, less y Sass**: vs 2013 se incluye sin selector de URL fuera de las páginas de formularios Web Forms. El nuevo selector de URL para los editores HTML, Razor, CSS, LESS y Sass es un selector de escritura fluida sin cuadro de diálogo que entiende '.. ' y filtra las listas de archivos adecuadamente para etiquetas y vínculos IMG.

    ![Selector de URL para etiqueta de imagen](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image16.png)    ![Selector de URL para vistas](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image17.png)    ![Selector de URL para CSS](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image18.png)

<a id="browserlink"></a>
### <a name="browser-link"></a>Vínculo con exploradores

- El vínculo del explorador ahora es compatible con las conexiones HTTPS y lo mostrará en el panel con otras conexiones, siempre que el certificado sea de confianza para el explorador.
- Asignación de origen HTML estático
- SPA compatibilidad con la asignación de datos
- Actualización automática de los datos de asignación

<a id="waws"></a>
### <a name="support-for-azure-app-service-web-apps-in-visual-studio"></a>Compatibilidad con Web Apps de Azure App Service en Visual Studio

- **Compatibilidad con el inicio de sesión de Azure.**
- **Depuración remota y vista remota para Web Apps**: ahora se admite la [depuración remota para aplicaciones web en Azure App Service](https://docs.microsoft.com/azure/app-service-web/web-sites-dotnet-troubleshoot-visual-studio) y la vista remota de archivos de contenido de la aplicación web en el explorador de servidores.

<a id="AzureResources"></a>
### <a name="create-remote-azure-resources-when-creating-a-new-web-project"></a>Crear recursos remotos de Azure al crear un nuevo proyecto web

En el cuadro de diálogo Nueva aplicación web se ha agregado la casilla ["crear recursos remotos"](https://docs.microsoft.com/azure/app-service-web/app-service-web-get-started-dotnet) de Azure. Al elegirlo, podrá integrar la experiencia de creación de una nueva aplicación Web, la configuración del sitio de publicación de Azure para pruebas y la creación de un perfil de publicación en unos pocos pasos sencillos.

![Nuevo proyecto con recursos de Azure](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image19.png)![Publicación en Azure](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image20.png)

<a id="webpublish"></a>
### <a name="web-publish-enhancements"></a>Mejoras en la publicación en Web

- Mejorar la experiencia del usuario para la publicación.

<a id="scaffolding"></a>
### <a name="aspnet-scaffolding"></a>Scaffolding de ASP.NET

- **Compatibilidad con enumeración:** Si el modelo usa enumeraciones, el scaffolding de MVC generará una lista desplegable para la enumeración. Usa las aplicaciones auxiliares de enumeración en MVC.
- **Compatibilidad con bootstrap**: se han actualizado las plantillas de EditorFor en scaffolding de MVC para que usen las clases bootstrap.
- **Compatibilidad con paquetes**: los scaffolding de MVC y Web API agregarán paquetes 5,1 para MVC y Web API

Las capturas de pantallas siguientes muestran los modelos de scaffolding.

- Código del modelo:

     ![Código del modelo](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image21.png)
- Compile el código del modelo, haga clic con el botón derecho y seleccione **Agregar**, **nuevo elemento con scaffolding**.

     ![Agregar nuevo elemento con scaffolding](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image22.png)
- Elija **controlador de MVC5 con vistas mediante Entity Framework**:

     ![Agregar nuevo controlador de MVC5 con vistas](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image23.png)
- **Agregue el controlador** mediante el modelo:

    ![](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image24.png)
- Compruebe el código generado, por ejemplo, views/WeekdayModels/Edit. cshtml contiene `@Html.EnumDropDownListFor`: ![vista que contiene EnumDropDownListFor](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image25.png)
- Ejecute la página para ver el cuadro combinado de enumeración generado. tenga en cuenta que si un valor puede ser null, se puede elegir una cadena vacía para el cuadro combinado. Por ejemplo, la página **crear** muestra lo siguiente:

    ![Cuadro combinado que permite una cadena vacía](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image26.png)

<a id="nuget"></a>
### <a name="nuget-281"></a>NuGet 2.8.1

NuGet 2.8.1 RTM se publicará en abril de 2014. Estos son los puntos destacados de las notas de la versión, pero consulte las notas de la [versión completa](http://docs.nuget.org/docs/release-notes/nuget-2.8) para obtener más información sobre estos cambios.

- **Aplicaciones Windows Phone 8,1 de destino**: ahora, 2.8.1 de NuGet admite aplicaciones de Windows Phone 8,1 de destino que usan los monikers de la plataforma de destino ' WindowsPhoneApp ', ' WPA ', ' WindowsPhoneApp81 ' y ' WPA81 '.

- **Resolución de revisión para las dependencias**: cuando se resuelven las dependencias de paquete, NuGet ha implementado históricamente una estrategia de selección de la versión de paquete principal y secundaria más antigua que satisface las dependencias del paquete. A diferencia de la versión principal y secundaria, sin embargo, la versión de revisión siempre se resolvió en la versión más alta. Aunque el comportamiento estaba bien intencionado, creó una falta de determinismo para instalar paquetes con dependencias.
- **Modificador DependencyVersion**: aunque NuGet 2,8 cambia el comportamiento *predeterminado* para resolver dependencias, también agrega un control más preciso sobre el proceso de resolución de dependencias a través del conmutador-DependencyVersion en la consola del administrador de paquetes. El modificador permite resolver las dependencias con la versión más baja posible (comportamiento predeterminado), la versión más alta posible o la versión secundaria o de revisión más alta. Este modificador solo funciona para Install-Package en el comando de PowerShell.
- **Atributo DependencyVersion**: además del modificador-DependencyVersion descrito anteriormente, NuGet también permite establecer un nuevo atributo en el archivo NuGet. config que define cuál es el valor predeterminado, si el modificador-DependencyVersion no se especifica en una invocación de Install-Package. Este valor también lo respeta el cuadro de diálogo Administrador de paquetes NuGet para cualquier operación de paquete de instalación. Para establecer este valor, agregue el atributo siguiente al archivo Nuget. config:

    `<config> <add key="dependencyversion" value="Highest" /> </config>`
- **Vista previa de las operaciones de Nuget con-Whatif**: algunos paquetes NuGet pueden tener gráficos de dependencia profundos y, por tanto, pueden ser útiles durante una operación de instalación, desinstalación o actualización para ver primero lo que ocurrirá. NuGet 2,8 agrega el modificador de PowerShell estándar (What if) a los comandos Install-Package, uninstall-Package y Update-package para habilitar la visualización del cierre completo de los paquetes a los que se aplicará el comando.
- **Degradar paquete**: no es raro instalar una versión preliminar de un paquete con el fin de investigar nuevas características y, a continuación, decidir revertir a la última versión estable. Antes de NuGet 2,8, se trata de un proceso de varios pasos para desinstalar el paquete de versión preliminar y sus dependencias, y, a continuación, instalar la versión anterior. Con NuGet 2,8, sin embargo, el paquete de actualización revertirá ahora todo el cierre del paquete (por ejemplo, el árbol de dependencias del paquete) a la versión anterior.
- **Dependencias de desarrollo**: muchos tipos diferentes de funcionalidades se pueden entregar como paquetes NuGet, incluidas las herramientas que se usan para optimizar el proceso de desarrollo. Estos componentes, aunque pueden ser instrumental para desarrollar un nuevo paquete, no deben considerarse una dependencia del nuevo paquete cuando se publica posteriormente. NuGet 2,8 permite a un paquete identificarse en el archivo. nuspec como developmentDependency. Cuando se instalan, estos metadatos también se agregan al archivo packages. config del proyecto en el que se instaló el paquete. Cuando el archivo packages. config se analice posteriormente para las dependencias de NuGet durante el paquete Nuget. exe, excluirá las dependencias marcadas como dependencias de desarrollo.
- **Archivos packages. config individuales para distintas plataformas**: al desarrollar aplicaciones para varias plataformas de destino, es habitual tener distintos archivos de proyecto para cada uno de los entornos de compilación respectivos. También es habitual consumir distintos paquetes de NuGet en diferentes archivos de proyecto, ya que los paquetes tienen distintos niveles de compatibilidad con distintas plataformas. NuGet 2,8 proporciona compatibilidad mejorada para este escenario creando distintos archivos packages. config para diferentes archivos de proyecto específicos de la plataforma.
- **Reserva a la memoria caché local**: aunque los paquetes Nuget suelen usarse desde una galería remota, como la [Galería de Nuget](http://www.nuget.org) mediante una conexión de red, hay muchos escenarios en los que el cliente no está conectado. Sin una conexión de red, el cliente de NuGet no pudo instalar paquetes correctamente, incluso cuando esos paquetes ya estaban en el equipo del cliente en la caché de NuGet local. NuGet 2,8 agrega la reserva de caché automática en la consola del administrador de paquetes.

    La característica de reserva de caché no requiere ningún argumento de comando específico. Además, la reserva de caché solo funciona actualmente en la consola del administrador de paquetes: el comportamiento no funciona actualmente en el cuadro de diálogo Administrador de paquetes.
- **Correcciones de errores**: una de las correcciones de errores más importantes realizadas fue la mejora del rendimiento en el comando Update-package-REINSTALL.

    Además de estas características y de la corrección de rendimiento mencionada anteriormente, esta versión de NuGet también incluye muchas otras correcciones de errores. Se han solucionado 181 problemas totales en la versión. Para obtener una lista completa de los elementos de trabajo corregidos en NuGet 2,8, consulte el [seguimiento de problemas de Nuget](https://nuget.codeplex.com/workitem/list/advanced?release=NuGet%202.8&status=all) en esta versión.

<a id="webforms"></a>
### <a name="aspnet-web-forms"></a>Formularios Web Forms de ASP.NET

- Ahora, las plantillas de formularios Web Forms muestran cómo realizar la confirmación de la cuenta y el restablecimiento de contraseña para ASP.NET Identity.
- El control de origen de datos de entidad y el proveedor de datos dinámicos para Entity Framework 6. Para obtener más información, vea el siguiente blog de MSDN: [proveedor de datos dinámicos y control EntityDataSource para Entity Framework 6](https://blogs.msdn.com/b/webdev/archive/2014/01/30/announcing-preview-of-dynamic-data-provider-and-entitydatasource-control-for-entity-framework-6.aspx).

<a id="mvc"></a>
### <a name="aspnet-mvc-512"></a>ASP.NET MVC 5.1.2

- [Mejoras en el enrutamiento de atributos](../../../mvc/overview/releases/mvc51-release-notes.md#AttributeRouting)
- [Compatibilidad de bootstrap con las plantillas de editor](../../../mvc/overview/releases/mvc51-release-notes.md#Bootstrap)
- [Compatibilidad de enumeración en vistas](../../../mvc/overview/releases/mvc51-release-notes.md#Enum)
- [Compatibilidad discreta para los atributos MinLength/MaxLength](../../../mvc/overview/releases/mvc51-release-notes.md#Unobtrusive)
- [Compatibilidad con el contexto ' this ' en Ajax discretos](../../../mvc/overview/releases/mvc51-release-notes.md#thisContext)
- Varias [correcciones de errores](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&status=Closed&type=All&priority=All&release=v5.1%20Preview%7cv5.1%20RTM&assignedTo=All&component=MVC&sortField=AssignedTo&sortDirection=Ascending&page=0&reasonClosed=Fixed)

<a id="webapi"></a>
### <a name="aspnet-web-api-212"></a>ASP.NET Web API 2.1.2

- [Control de errores global](../../../web-api/overview/releases/whats-new-in-aspnet-web-api-21.md#global-error)
- [Mejoras en el enrutamiento de atributos](../../../web-api/overview/releases/whats-new-in-aspnet-web-api-21.md#attribute-routing)
- [Mejoras en la página de ayuda](../../../web-api/overview/releases/whats-new-in-aspnet-web-api-21.md#help-page)
- [Compatibilidad con IgnoreRoute](../../../web-api/overview/releases/whats-new-in-aspnet-web-api-21.md#ignoreroute)
- [Formateador de tipo multimedia BSON](../../../web-api/overview/releases/whats-new-in-aspnet-web-api-21.md#bson)
- [Mejor compatibilidad con filtros Async](../../../web-api/overview/releases/whats-new-in-aspnet-web-api-21.md#async-filters)
- [Análisis de consultas para la biblioteca de formato de cliente](../../../web-api/overview/releases/whats-new-in-aspnet-web-api-21.md#query-parsing)
- Varias [correcciones de errores](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&status=Closed&type=All&priority=All&release=v5.1%20Preview%7cv5.1%20RTM&assignedTo=All&component=Web%20API%7cWeb%20API%20OData&sortField=AssignedTo&sortDirection=Ascending&page=0&reasonClosed=Fixed)

<a id="webpages"></a>
### <a name="aspnet-web-pages-312"></a>ASP.NET Web Pages 3.1.2

- Varias [correcciones de errores](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&status=Closed&type=All&priority=All&release=v5.1%20Preview%7cv5.1%20RTM&assignedTo=All&component=Web%20Pages/Razor&sortField=AssignedTo&sortDirection=Ascending&page=0&reasonClosed=Fixed)

<a id="ef"></a>
### <a name="entity-framework-61"></a>Entity Framework 6,1

Entity Framework se ha actualizado a la versión 6,1 para el tiempo de ejecución y las herramientas. Entity Framework (EF) 6,1 es una actualización secundaria de Entity Framework 6 e incluye una serie de correcciones de errores y nuevas características. Para obtener información detallada sobre EF 6.1, incluidos los vínculos a la documentación de las nuevas características, vea [Entity Framework historial de versiones](https://msdn.microsoft.com/data/jj574253). Las nuevas características de esta versión incluyen:

- La **consolidación de herramientas** proporciona una manera coherente de crear un nuevo modelo EF. Esta característica amplía el Asistente para Entity Data Model de ADO.NET para admitir la creación de modelos Code First, incluida la ingeniería inversa de una base de datos existente. Estas características estaban previamente disponibles en calidad beta en las herramientas avanzadas de EF.
- El **control de los errores de confirmación de la transacción** proporciona el nuevo [System. Data. Entity. Infrastructure. CommitFailureHandler](https://msdn.microsoft.com/library/system.data.entity.infrastructure.commitfailurehandler(v=vs.113).aspx) , que hace uso de la capacidad incorporada recientemente para interceptar las operaciones de transacción. **CommitFailureHandler** permite la recuperación automática de errores de conexión mientras se confirma una transacción.
- **IndexAttribute** permite especificar los índices colocando un atributo en una propiedad (o propiedades) en el modelo de Code First. A continuación, Code First creará un índice correspondiente en la base de datos.
- **La API de asignación pública** proporciona acceso a la información EF tiene sobre cómo se asignan las propiedades y los tipos a las columnas y tablas de la base de datos. En las versiones anteriores, esta API era interna.
- **Capacidad de configurar los interceptores a través del archivo App/web. config**(permitiendo que los interceptores se agreguen sin volver a compilar la aplicación).
- **DatabaseLogger** es un nuevo interceptor que facilita el registro de todas las operaciones de base de datos en un archivo. En combinación con la característica anterior, esto le permite cambiar fácilmente el registro de las operaciones de base de datos para una aplicación implementada, sin necesidad de volver a compilar.
- La **detección de cambios del modelo de migración** se ha mejorado para que las migraciones con scaffolding sean más precisas. también se ha mejorado enormemente el rendimiento del proceso de detección de cambios.
- **Mejoras** en el rendimiento, incluidas las operaciones de base de datos reducidas durante la inicialización, optimizaciones para la comparación de igualdad nula en consultas LINQ, generación más rápida de vistas (creación de modelos) en más escenarios y materialización más eficaz de las entidades sometidas a seguimiento con varias asociaciones.

<a id="identity"></a>
### <a name="aspnet-identity-200"></a>ASP.NET Identity 2.0.0

- **Autenticación en dos fases**: ASP.net Identity admite ahora la autenticación en dos fases. La autenticación en dos fases proporciona una capa adicional de seguridad a las cuentas de usuario en caso de que la contraseña se vea comprometida. También hay protección para ataques por fuerza bruta contra los códigos de dos factores.
- **Bloqueo de cuenta:** Proporciona una manera de bloquear al usuario si el usuario escribe la contraseña o los códigos de dos factores de forma incorrecta. Se puede configurar el número de intentos no válidos y el intervalo de tiempo de los usuarios bloqueados. Opcionalmente, un desarrollador puede desactivar el bloqueo de cuentas para ciertas cuentas de usuario en caso de que sea necesario.
- **Confirmación de la cuenta:** El sistema ASP.NET Identity admite ahora la confirmación de la cuenta. Este es un escenario bastante común en la mayoría de los sitios web hoy en los que, cuando se registra para una cuenta nueva en el sitio web, es necesario confirmar el correo electrónico antes de poder hacer nada en el sitio Web. La confirmación por correo electrónico es útil porque evita que se creen cuentas falsas. Esto resulta muy útil si usa el correo electrónico como método de comunicación con los usuarios de su sitio web, como sitios de foros, bancos, comercio electrónico o sitios web sociales.
- **Restablecimiento de contraseña:** El restablecimiento de contraseña es una característica en la que el usuario puede restablecer sus contraseñas si ha olvidado su contraseña.
- **Marca de seguridad (cerrar sesión en todas partes):** Admite una manera de volver a generar el token de seguridad para el usuario en los casos en que el usuario cambia la contraseña o cualquier otra información relacionada con la seguridad, como quitar un inicio de sesión asociado (como Facebook, Google, cuenta de Microsoft, etc.). Esto es necesario para asegurarse de que se invalidan los tokens generados con la contraseña antigua. En el proyecto de ejemplo, si cambia la contraseña del usuario, se genera un nuevo token para el usuario y se invalidan los tokens anteriores. Esta característica proporciona un nivel de seguridad adicional a la aplicación, ya que cuando se cambia la contraseña, se cerrará la sesión desde cualquier lugar (todos los demás exploradores) en el que haya iniciado sesión en esta aplicación.
- **Haga que el tipo de clave principal sea extensible para usuarios y roles**: en ASP.net Identity 1,0, el tipo de clave principal para los roles y usuarios de la tabla era cadenas. Esto significa que cuando el sistema de ASP.NET Identity se almacenó en SQL Server con Entity Framework, se usaba nvarchar. Hay muchas discusiones en torno a esta implementación predeterminada en Stack Overflow y en función de los comentarios entrantes. Hemos proporcionado un enlace de extensibilidad en el que puede especificar qué debe ser la clave principal de la tabla de usuarios y roles. Este enlace de extensibilidad es especialmente útil si está migrando la aplicación y la aplicación estaba almacenando los identificadores de usuario en GUID o ints.
- **Compatibilidad con IQueryable en usuarios y roles**: se ha agregado compatibilidad con IQueryable en UsersStore y RolesStore, se puede obtener fácilmente la lista de usuarios y roles.
- **Compatibilidad con la operación de eliminación a través de UserManager**
- **Indexación en el nombre de usuario**: en ASP.net Identity implementación de Entity Framework, hemos agregado un índice único en el nombre de usuario con el nuevo INDEXATTRIBUTE en EF 6.1.0. Esto garantiza que los nombres de usuario sean siempre únicos y no haya ninguna condición de carrera en la que pueda acabar con los nombres de usuario duplicados.
- **Validador de contraseñas mejorado:** El validador de contraseñas que se envió en ASP.NET Identity 1,0 era un validador de contraseña bastante básico que solo validaba la longitud mínima. Hay un nuevo validador de contraseña que le proporciona más control sobre la complejidad de la contraseña. Tenga en cuenta que incluso si activa todos los valores de esta contraseña, le animamos a habilitar la autenticación en dos fases para las cuentas de usuario.
- **Middleware IdentityFactory/CreatePerOwinContext**:

    - **Administrador de usuarios**: puede usar la implementación de fábrica para obtener una instancia de UserManager desde el contexto OWIN. Este patrón es similar a lo que usamos para obtener AuthenticationManager del contexto OWIN para inicio de sesión y cierre de sesión. Se trata de una manera recomendada de obtener una instancia de UserManager por solicitud de la aplicación.
    - **DbContextFactory**: ASP.NET Identity usa Entity Framework para conservar el sistema de identidades en SQL Server. Para ello, el sistema de identidades tiene una referencia a ApplicationDbContext. El middleware DbContextFactory devuelve una instancia de ApplicationDbContext por solicitud que se puede usar en la aplicación.
- **Paquete Nuget de ejemplos de ASP.net Identity**: el paquete Nuget de ejemplos puede facilitar la instalación y ejecución de ejemplos para ASP.net Identity y seguir las prácticas recomendadas. Se trata de una aplicación ASP.NET MVC de ejemplo. Modifique el código para que se adapte a su aplicación antes de implementarlo en producción. El ejemplo debe instalarse en una aplicación ASP.NET vacía. Para obtener más información sobre el paquete, vaya a la siguiente entrada de blog: [anuncio de RTM de ASP.net Identity 2.0.0](https://blogs.msdn.com/b/webdev/archive/2014/03/20/test-announcing-rtm-of-asp-net-identity-2-0-0.aspx)

<a id="owin"></a>
### <a name="microsoft-owin-components"></a>Componentes OWIN de Microsoft

Se han corregido muchos errores en esta versión. Consulte las [notas de la versión de la versión 2.1.0](https://katanaproject.codeplex.com/releases/view/113281) para obtener información más detallada.

<a id="signalr"></a>
### <a name="aspnet-signalr-202"></a>ASP.NET Signalr 2.0.2

Se han corregido muchos errores en esta versión. Consulte las [notas de la versión de la versión 2.0.2](https://github.com/SignalR/SignalR/releases/tag/2.0.2) para obtener información más detallada.
