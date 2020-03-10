---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/create-the-project
title: Cree el proyecto | Microsoft Docs
author: Erikre
description: Esta serie de tutoriales le enseñará los aspectos básicos de la creación de una aplicación de formularios Web Forms ASP.NET con ASP.NET 4,5 y Microsoft Visual Studio Express 2013 para nosotros...
ms.author: riande
ms.date: 09/08/2014
ms.assetid: 2ce36f78-8ecb-4ab1-b748-6d0ab633ea3f
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/create-the-project
msc.type: authoredcontent
ms.openlocfilehash: 62918b17f42e54dfe4e45a08927b1039dcbb7012
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78461575"
---
# <a name="create-the-project"></a>Crear el proyecto

por [Erik Reitan](https://github.com/Erikre)

[Descargar el proyecto de ejemplo deC#Wingtip Toys ()](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) o [descargar el libro electrónico (pdf)](https://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)

> Esta serie de tutoriales le enseñará los aspectos básicos de la creación de una aplicación de formularios Web Forms ASP.NET con ASP.NET 4,5 y Microsoft Visual Studio Express 2013 para Web. Hay disponible un [proyecto C# de Visual Studio 2013 con código fuente](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) para acompañar esta serie de tutoriales.

En este tutorial, creará, revisará y ejecutará el proyecto predeterminado en Visual Studio, lo que le permitirá familiarizarse con las características de ASP.NET. Además, revisará el entorno de Visual Studio.

## <a name="what-youll-learn"></a>Temas que se abordarán:

- Cómo crear un nuevo proyecto de formularios Web Forms.
- La estructura de archivos del proyecto de formularios Web Forms.
- Cómo ejecutar el proyecto en Visual Studio.
- Las diferentes características de la aplicación de formularios Web Forms predeterminada.
- Algunos aspectos básicos sobre cómo usar el entorno de Visual Studio.

## <a name="creating-the-project"></a>Crear el proyecto

1. Abra Visual Studio.
2. Seleccione **nuevo proyecto** en el menú **archivo** de Visual Studio. 

    ![Crear el elemento de menú proyecto-nuevo proyecto](create-the-project/_static/image1.png)
3. Seleccione el grupo **plantillas -&gt;**  **C# visual** -&gt; plantillas **Web** de la izquierda.
4. Elija la plantilla **Aplicación web ASP.NET** en la columna central.  
 Esta serie de tutoriales utiliza .NET Framework 4.5.2.
5. Asigne al proyecto el nombre *WingtipToys* y elija el botón **Aceptar** . 

    ![Crear el cuadro de diálogo proyecto-nuevo proyecto](create-the-project/_static/image2.png)

    > [!NOTE]
    > El nombre del proyecto en esta serie de tutoriales es **WingtipToys**. Se recomienda usar este nombre de proyecto *exacto* para que el código proporcionado en toda la serie de tutoriales funcione según lo esperado.

6. Haga clic en el botón **Cambiar autenticación**. Seleccione **cuentas de usuario individuales** y haga clic en el botón **Aceptar** .

7. Seleccione la plantilla de **formularios Web Forms** y haga clic en el botón **Aceptar** .

    ![Crear la plantilla proyecto-nuevo proyecto](create-the-project/_static/image3.png)

El proyecto tardará un poco en crearse. Cuando esté listo, abra la página **default. aspx** .

![Crear la plantilla proyecto-nuevo proyecto](create-the-project/_static/image4.png)

Puede cambiar entre la vista de **diseño** y la vista de **código fuente** seleccionando una opción en la parte inferior de la ventana central. La vista de **diseño** muestra ASP.net páginas web, páginas maestras, páginas de contenido, páginas HTML y controles de usuario mediante una vista casi WYSIWYG. Vista **código fuente** muestra el formato HTML de la página web, que se puede editar.

> [!TIP] 
> 
> **Descripción de los marcos de trabajo de ASP.NET**
> 
> Formularios web de ASP.NET permite crear sitios web dinámicos con un modelo familiar controlado por eventos de arrastrar y colocar. Una superficie diseño y cientos de controles y componentes le permiten crear rápidamente sitios potentes y sofisticados sitios controlados por IU con datos. El almacén Wingtip Toy se basa en formularios Web Forms ASP.NET, pero muchos de los conceptos que aprenderá en esta serie de tutoriales son aplicables a todos los ASP.NET.
> 
> ASP.NET ofrece cuatro marcos de desarrollo principales:
> 
> - [Formularios Web Forms ASP.NET](../../../index.md)  
>  El marco de trabajo de formularios Web Forms se dirige a los desarrolladores que prefieren la programación declarativa y basada en controles, como Microsoft Windows Forms (WinForms) y WPF/XAML/Silverlight. Ofrece un modelo de desarrollo basado en diseñadores WYSIWYG, por lo que es popular con los desarrolladores que buscan un entorno de desarrollo rápido de aplicaciones (RAD) para el desarrollo web. Si no está familiarizado con la programación web y está familiarizado con las herramientas de desarrollo de cliente de Microsoft RAD tradicionales (por C#ejemplo, para Visual Basic y visual), puede compilar rápidamente una aplicación web sin tener experiencia en HTML y JavaScript.
> - [ASP.NET MVC](../../../../mvc/index.md) (Más información sobre ASP.NET MVC)  
>  ASP.NET MVC está dirigido a desarrolladores interesados en patrones y principios como el desarrollo controlado por pruebas, la separación de preocupaciones, la inversión de control (IoC) y la inserción de dependencias (DI). Este marco de trabajo fomenta la separación del nivel de lógica de negocios de una aplicación web del nivel de presentación.
> - [ASP.NET Web Pages](../../../../web-pages/index.md) (Más información sobre páginas web de ASP.NET)  
>  ASP.NET Web Pages se dirige a los desarrolladores que quieren un caso de desarrollo web simple, a lo largo de las líneas de PHP. En el modelo de páginas web, cree páginas HTML y, a continuación, agregue código basado en servidor a la página para controlar de forma dinámica cómo se representa el marcado. Páginas web está diseñada específicamente para ser un marco de trabajo ligero, y es el punto de entrada más sencillo para las personas que conocen HTML, pero que podrían no tener una amplia experiencia de programación (por ejemplo, estudiantes o aficionados). También es una buena forma de que los desarrolladores web que saben que PHP o marcos similares empiecen a usar ASP.NET.
> - [Aplicación de una sola página de ASP.NET](../../../../single-page-application/index.md)  
>  ASP.NET aplicación de una sola página (SPA) le ayuda a crear aplicaciones que incluyen interacciones del lado cliente significativas con HTML 5, CSS 3 y JavaScript. La actualización ASP.NET and Web Tools 2012,2 envía una plantilla nueva para compilar aplicaciones de una sola página mediante Knockout. js y ASP.NET Web API. Además de la nueva plantilla de SPA, también se pueden descargar nuevas plantillas de SPA creadas por la comunidad.
> 
> Además de los cuatro marcos de desarrollo principales, ASP.NET también ofrece tecnologías adicionales que es importante tener en cuenta y que están familiarizados con, pero que no se describen en esta serie de tutoriales:
> 
> - [ASP.net web API](../../../../web-api/index.md) : un marco de trabajo para la creación de servicios http que llegan a una amplia gama de clientes, incluidos exploradores y dispositivos móviles.
> - [ASP.net signalr](../../../../signalr/index.md) : biblioteca que facilita el desarrollo de la funcionalidad web en tiempo real.

### <a name="reviewing-the-project"></a>Revisar el proyecto

En Visual Studio, la ventana de **Explorador de soluciones** permite administrar archivos para el proyecto. Echemos un vistazo a las carpetas que se han agregado a la aplicación en **Explorador de soluciones**. La plantilla aplicación web agrega una estructura de carpetas básica:

![Crear el proyecto-Explorador de soluciones](create-the-project/_static/image5.png)

Visual Studio crea algunas carpetas y archivos iniciales para el proyecto. Los primeros archivos con los que va a trabajar más adelante en este tutorial son los siguientes:

| **Archivo** | **Propósito** |
| --- | --- |
| *Default. aspx* | Normalmente, la primera página que se muestra cuando la aplicación se ejecuta en un explorador. |
| *Site. Master* | Una página que le permite crear un diseño coherente y usar el comportamiento estándar para las páginas de la aplicación. |
| *Global. asax* | Un archivo opcional que contiene código para responder a los eventos de nivel de aplicación y de sesión generados por ASP.NET o por módulos HTTP. |
| *Web.config* | Los datos de configuración de una aplicación. |

### <a name="running-the-default-web-application"></a>Ejecutar la aplicación web predeterminada

La aplicación web predeterminada proporciona una experiencia enriquecida basada en la funcionalidad y compatibilidad integradas. Sin realizar cambios en el proyecto de formularios Web Forms predeterminado, la aplicación está lista para ejecutarse en el explorador Web local.

1. Presione la tecla ***F5*** mientras se está en Visual Studio.   
 La aplicación se compilará y mostrará en el explorador Web.  

    ![Crear la página predeterminada del proyecto](create-the-project/_static/image6.png)
2. Una vez que haya completado la revisión de la aplicación en ejecución, cierre la ventana del explorador.

Hay tres páginas principales en esta aplicación web predeterminada: *default. aspx* (Home), *About. aspx*y *Contact. aspx*. Se puede acceder a cada una de estas páginas desde la barra de navegación superior. También hay dos páginas adicionales contenidas en la carpeta de la cuenta, la página Register. aspx y la página login. aspx. Estas dos páginas permiten usar las capacidades de pertenencia de ASP.NET para crear, almacenar y validar las credenciales de usuario.

## <a name="aspnet-web-forms-background"></a>ASP.NET fondo de formularios Web Forms

Los formularios Web Forms ASP.NET son páginas basadas en Microsoft ASP.NET tecnología, en las que el código que se ejecuta en el servidor genera dinámicamente la salida de la página web en el explorador o el dispositivo cliente. Una página de formularios Web Forms de ASP.NET representa automáticamente el HTML compatible con el explorador correcto para características como estilos, diseño, etc. Los formularios Web Forms son compatibles con cualquier lenguaje compatible con .NET Common Language Runtime, como Microsoft Visual Basic y Microsoft C#visual. Además, los formularios Web Forms se basan en el [marco de Microsoft .net](https://msdn.microsoft.com/vstudio/aa496123), que ofrece ventajas como un entorno administrado, la seguridad de tipos y la herencia.

Cuando se ejecuta una página de formularios Web Forms de ASP.NET, la página pasa por un ciclo de vida en el que realiza una serie de pasos de procesamiento. Estos pasos incluyen la inicialización, la creación de instancias de controles, la restauración y el mantenimiento del estado, la ejecución del código del controlador de eventos y la representación. A medida que se familiarice con la eficacia de los formularios Web Forms de ASP.NET, es importante comprender el [ciclo de vida](https://msdn.microsoft.com/library/ms178472(v=vs.100).aspx) de la página de ASP.net para que pueda escribir código en la fase de ciclo de vida adecuada para el efecto que desea.

Cuando un servidor web recibe una solicitud para una página, encuentra la página, la procesa, la envía al explorador y, a continuación, descarta toda la información de la página. Si el usuario solicita la misma página de nuevo, el servidor repite toda la secuencia y vuelve a procesar la página desde cero. En otro caso, un servidor no tiene memoria de páginas que ha procesado; las páginas no tienen estado. El marco de trabajo de la página ASP.NET controla automáticamente la tarea de mantener el estado de la página y sus controles, y proporciona métodos explícitos para mantener el estado de la información específica de la aplicación.

> [!TIP] 
> 
> **Características de la aplicación web en la plantilla aplicación de formularios Web Forms**
> 
> La plantilla de aplicación de formularios Web Forms ASP.NET ofrece un amplio conjunto de funciones integradas. No solo proporciona una página *Home. aspx* , una página *About. aspx* , una página *Contact. aspx* , sino que también incluye funcionalidad de pertenencia que registra usuarios y guarda sus credenciales para que puedan iniciar sesión en su sitio Web. En esta información general se proporciona más información sobre algunas de las características incluidas en la plantilla aplicación de formularios Web Forms de ASP.NET y cómo se usan en la aplicación Wingtip Toys.
> 
> **Pertenencia**
> 
> [ASP.net](https://msdn.microsoft.com/library/yh26yfzy.aspx) La identidad almacena las credenciales de los usuarios en una base de datos creada por la aplicación. Cuando los usuarios inician sesión, la aplicación valida las credenciales mediante la lectura de la base de datos. La carpeta de la *cuenta* del proyecto contiene los archivos que implementan las distintas partes de la pertenencia: registro, Inicio de sesión, cambio de contraseña y autorización de acceso. Además, los formularios Web Forms de ASP.NET admiten OAuth y OpenID. Estas mejoras de autenticación permiten a los usuarios iniciar sesión en el sitio con las credenciales existentes, desde cuentas como Facebook, Twitter, Windows Live y Google.
> 
> ![Crear el proyecto Explorador de soluciones (ASP.NET Identity)](create-the-project/_static/image7.png)
> 
> De forma predeterminada, la plantilla crea una base de datos de pertenencia con un nombre de base de datos predeterminado en una instancia de SQL Server Express LocalDB, el servidor de base de datos de desarrollo que viene con Visual Studio Express 2013 para Web.
> 
> **SQL Server Express LocalDB**
> 
> [SQL Server Express LocalDB](https://technet.microsoft.com/library/hh510202.aspx) es una versión ligera de SQL Server que tiene muchas características de programación de una base de datos de SQL Server. SQL Server Express LocalDB se ejecuta en modo usuario y tiene una instalación rápida sin configuración que tiene una breve lista de requisitos previos de instalación. En Microsoft SQL Server, cualquier base de datos o código Transact-SQL se puede pasar de SQL Server Express LocalDB a SQL Server y SQL Azure sin ningún paso de actualización. Por lo tanto, SQL Server Express LocalDB se puede usar como un entorno de desarrollo para aplicaciones destinadas a todas las ediciones de SQL Server. SQL Server Express LocalDB habilita características como procedimientos almacenados, funciones definidas por el usuario y agregados, .NET Framework integración, tipos espaciales y otros que no están disponibles en SQL Server Compact.
> 
> **Páginas maestras**
> 
> Una [página maestra de ASP.net](https://msdn.microsoft.com/library/wtxbf3hh.aspx) define un aspecto y un comportamiento coherentes para todas las páginas de la aplicación. El diseño de la página maestra se combina con el contenido de una página de contenido individual para generar la página final que el usuario ve. En la aplicación Wingtip Toys, modifique la página maestra *site. Master* para que todas las páginas del sitio web de Wingtip Toys compartan el mismo logotipo y la misma barra de navegación.
> 
> **HTML5**
> 
> La plantilla de aplicación de formularios Web Forms de ASP.NET admite [HTML5](http://www.w3schools.com/html/html5_intro.asp), que es la versión más reciente del lenguaje de marcado HTML. HTML5 admite nuevos elementos y funcionalidades que facilitan la creación de sitios Web.
> 
> **Modernizr**
> 
> Para los exploradores que no son compatibles con HTML5, puede usar [modernizr](http://www.modernizr.com/). Modernizr es una biblioteca de JavaScript de código abierto que puede detectar si un explorador admite características de HTML5 y habilitarlos si no es así. En la plantilla de aplicación de formularios Web Forms ASP.NET, Modernizr se instala como un paquete NuGet.
> 
> **Bootstrap**
> 
> Las plantillas de proyecto de Visual Studio 2013 usan [bootstrap](http://getbootstrap.com/), un diseño y un marco de trabajo de creación de proyectos creados por Twitter. Bootstrap usa CSS3 para proporcionar un diseño dinámico, lo que significa que los diseños pueden adaptarse dinámicamente a distintos tamaños de ventana del explorador. También puede usar la característica de las funciones de bootstrap para aplicar fácilmente un cambio en la apariencia y el funcionamiento de la aplicación. De forma predeterminada, la plantilla de aplicación Web ASP.NET en Visual Studio 2013 incluye bootstrap como un paquete NuGet.
> 
> **Paquetes NuGet**
> 
> La plantilla de aplicación de formularios Web Forms ASP.NET incluye un conjunto de paquetes [NuGet](http://www.nuget.org/) . Estos paquetes proporcionan funcionalidad en componentes en forma de bibliotecas y herramientas de código abierto. Hay una amplia variedad de paquetes para ayudarle a crear y probar sus aplicaciones. Visual Studio facilita la tarea de agregar, quitar y actualizar paquetes de NuGet. Los desarrolladores también pueden crear y agregar paquetes a NuGet.
> 
> ![Crear el cuadro de diálogo proyecto-NuGet](create-the-project/_static/image8.png)
> 
> Al instalar un paquete, NuGet copia los archivos en la solución y realiza automáticamente los cambios necesarios, como agregar referencias y cambiar la configuración asociada a la aplicación Web. Si decide quitar la biblioteca, NuGet quita los archivos e invierte cualquier cambio que se haya realizado en el proyecto para que no quede ningún desorden. NuGet está disponible en el menú **herramientas** de Visual Studio.
> 
> **jQuery**
> 
> [jQuery](http://jquery.com/) es una biblioteca de JavaScript rápida y concisa que simplifica el recorrido de documentos HTML, el control de eventos, la animación y las interacciones de Ajax para un rápido desarrollo web. La biblioteca de JavaScript de jQuery se incluye en la plantilla de aplicación de formularios Web Forms de ASP.NET como un paquete NuGet.
> 
> **Validación discreta**
> 
> Los controles de validador integrados se han configurado para usar JavaScript discreto para la lógica de validación del lado cliente. Esto reduce considerablemente la cantidad de código JavaScript insertado en el marcado de la página y reduce el tamaño total de la página. La validación discreta se agrega globalmente a la plantilla de aplicación de formularios Web Forms de ASP.NET basándose en la configuración del elemento &lt;appSettings&gt; del archivo *Web. config* en la raíz de la aplicación.
> 
> **Entity Framework Code First**
> 
> Además de las características de la plantilla de aplicación de formularios Web Forms de ASP.NET, la aplicación Wingtip Toys usa [Entity Framework Code First](https://weblogs.asp.net/scottgu/archive/2010/12/08/announcing-entity-framework-code-first-ctp5-release.aspx), que es una biblioteca de NuGet que permite el desarrollo centrado en el código cuando se trabaja con datos. En pocas palabras, crea la parte de la base de datos de la aplicación en función del código que escriba. Con el Entity Framework, se recuperan y manipulan los datos como objetos fuertemente tipados. Esto le permite centrarse en la lógica de negocios de la aplicación en lugar de en los detalles de cómo se obtiene acceso a los datos.
> 
> Para obtener más información sobre las bibliotecas y los paquetes instalados que se incluyen con la plantilla de formularios Web Forms de ASP.NET, consulte la lista de paquetes de NuGet instalados. Para ello, en Visual Studio, cree un nuevo proyecto de formularios Web Forms, seleccione **herramientas** > **Administrador de paquetes Nuget** > **administrar paquetes Nuget para la solución**y seleccione **paquetes instalados** en el cuadro de diálogo **administrar paquetes Nuget** .

### <a name="touring-visual-studio"></a>Paseo por Visual Studio

Las ventanas principales de Visual Studio incluyen el **Explorador de soluciones**, el **Explorador de servidores** (**Explorador de bases de datos** en Express), la **ventana Propiedades**, el **cuadro de herramientas**, la **barra de herramientas**y la ventana de **documento**.

![Crear el cuadro de diálogo proyecto-NuGet](create-the-project/_static/image9.png)

Para obtener más información sobre Visual Studio, vea la [guía visual de Visual Web Developer](https://msdn.microsoft.com/library/ee410104.aspx).

## <a name="summary"></a>Resumen

En este tutorial ha creado, revisado y ejecutado la aplicación de formularios Web Forms predeterminada. Ha revisado las diferentes características de la aplicación de formularios Web Forms predeterminada y ha aprendido algunos aspectos básicos sobre cómo usar el entorno de Visual Studio. En los tutoriales siguientes, creará la capa de acceso a datos.

## <a name="additional-resources"></a>Recursos adicionales

[Elección del modelo de programación adecuado](../../../videos/how-do-i/choosing-the-right-programming-model.md)   
[Proyectos de aplicación web frente a proyectos de sitio web](https://msdn.microsoft.com/library/dd547590.aspx)   
[Información general de las páginas de formularios Web Forms ASP.NET](https://msdn.microsoft.com/library/428509ah.aspx)

> [!div class="step-by-step"]
> [Anterior](introduction-and-overview.md)
> [Siguiente](create_the_data_access_layer.md)
