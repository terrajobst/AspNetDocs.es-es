---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/determining-what-files-need-to-be-deployed-cs
title: Determinar qué archivos se deben implementar (C#) | Microsoft Docs
author: rick-anderson
description: Los archivos que se deben implementar desde el entorno de desarrollo en el entorno de producción dependen en parte de si se ha creado la aplicación ASP.NET...
ms.author: riande
ms.date: 04/01/2009
ms.assetid: f8d78a88-cc91-40d8-bce3-3d7954f6033b
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/determining-what-files-need-to-be-deployed-cs
msc.type: authoredcontent
ms.openlocfilehash: 1dd4a1179d32f776626c08a07205dc9aabed588d
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/28/2019
ms.locfileid: "74596377"
---
# <a name="determining-what-files-need-to-be-deployed-c"></a>Determinar qué archivos se deben implementar (C#)

por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descargar código](https://download.microsoft.com/download/4/5/F/45F815EC-8B0E-46D3-9FB8-2DC015CCA306/ASPNET_Hosting_Tutorial_02_CS.zip) o [Descargar PDF](https://download.microsoft.com/download/E/8/9/E8920AE6-D441-41A7-8A77-9EF8FF970D8B/aspnet_tutorial02_FilesToDeploy_cs.pdf)

> Los archivos que se deben implementar desde el entorno de desarrollo en el entorno de producción dependen en parte de si la aplicación ASP.NET se compiló con el modelo de sitio web o el modelo de aplicación Web. Obtenga más información sobre estos dos modelos de proyecto y el modo en que el modelo de proyecto afecta a la implementación.

## <a name="introduction"></a>Introducción

La implementación de una aplicación Web ASP.NET implica copiar los archivos relacionados con ASP.NET del entorno de desarrollo en el entorno de producción. Los archivos relacionados con ASP.NET incluyen el marcado de la Página Web de ASP.NET y el código y los archivos auxiliares del lado cliente y del servidor. Los archivos de compatibilidad del lado cliente son los archivos a los que hacen referencia las páginas web y se envían directamente a las imágenes del explorador, los archivos CSS y los archivos JavaScript, por ejemplo. Entre los archivos de compatibilidad del lado servidor se incluyen los que se usan para procesar una solicitud en el lado servidor. Esto incluye los archivos de configuración, los servicios Web, los archivos de clase, los conjuntos de valores de tipos y los archivos LINQ to SQL, entre otros.

En general, todos los archivos de compatibilidad del lado cliente deben copiarse desde el entorno de desarrollo al entorno de producción, pero los archivos de compatibilidad del servidor que se copian dependerán de si se compila explícitamente el código del lado servidor en un ensamblado (un archivo `.dll`) o si se tienen estos ensamblados generados automáticamente. En este tutorial se resaltan los archivos que se deben implementar al compilar explícitamente el código en un ensamblado en lugar de que este paso de compilación se produzca automáticamente.

## <a name="explicit-compilation-versus-automatic-compilation"></a>Compilación explícita frente a compilación automática

Las páginas web de ASP.NET se dividen en código fuente y marcado declarativo. La parte del marcado declarativo incluye HTML, controles Web y sintaxis de DataBinding; la parte de código contiene controladores de eventos escritos en Visual Basic C# o código. Las partes de código y marcado normalmente se separan en archivos diferentes: `WebPage.aspx` contiene el marcado declarativo mientras `WebPage.aspx.cs` hospeda el código.

Considere una página ASP.NET denominada Clock. aspx que contenga un control etiqueta cuya propiedad texto esté establecida en la fecha y hora actuales en que se carga la página. La parte de marcado declarativo (en `Clock.aspx`) incluiría el marcado de un control Web de etiqueta-`<asp:Label runat="server" id="TimeLabel" />`-mientras que la parte de código (en `Clock.aspx.cs`) tendría un controlador de eventos `Page_Load` con el código siguiente:

[!code-csharp[Main](determining-what-files-need-to-be-deployed-cs/samples/sample1.cs)]

Para que el motor de ASP.NET pueda atender una solicitud de esta página, primero se debe compilar la parte del código de la página (el archivo `WebPage.aspx.cs`). Esta compilación puede producirse de forma explícita o automática.

Si la compilación se produce explícitamente, el código fuente de la aplicación completa se compila en uno o varios ensamblados (archivos`.dll`) ubicados en el directorio de `Bin` de la aplicación. Si la compilación se produce automáticamente, el ensamblado generado automáticamente resultante se coloca de forma predeterminada en la carpeta `Temporary ASP.NET` archivos, que se encuentra en `%WINDOWS%\Microsoft.NET\Framework\` *&lt;versión&gt;* , aunque esta ubicación se puede configurar a través del [elemento`<compilation>`](https://msdn.microsoft.com/library/s10awwz0.aspx) en `Web.config`. Con la compilación explícita, debe realizar alguna acción para compilar el código de la aplicación ASP.NET en un ensamblado y este paso se produce antes de la implementación. Con la compilación automática, el proceso de compilación se produce en el servidor Web cuando se obtiene acceso por primera vez al recurso.

Independientemente del modelo de compilación que use, la parte de marcación de todas las páginas de ASP.NET (los archivos `WebPage.aspx`) debe copiarse en el entorno de producción. Con la compilación explícita, debe copiar los ensamblados en la carpeta `Bin`, pero no es necesario copiar las partes del código de las páginas ASP.NET (los archivos `WebPage.aspx.cs`). Con la compilación automática, debe copiar los archivos de parte de código para que el código esté presente y se pueda compilar automáticamente cuando se visite la página. La parte de marcado de cada página web de ASP.NET incluye una directiva de `@Page` con atributos que indican si el código asociado de la página ya se compiló explícitamente o si es necesario compilarlo automáticamente. Como resultado, el entorno de producción puede trabajar con cualquier modelo de compilación sin problemas y no es necesario aplicar ninguna configuración especial para indicar que se utiliza la compilación explícita o automática.

En la tabla 1 se resumen los distintos archivos que se van a implementar cuando se usa la compilación explícita en lugar de la compilación automática. Tenga en cuenta que, independientemente del modelo de compilación utilizado, siempre debe implementar los ensamblados en la carpeta `Bin`, si esa carpeta existe. La carpeta `Bin` contiene los ensamblados específicos de la aplicación Web, que incluyen el código fuente compilado al utilizar el modelo de compilación explícita. El directorio `Bin` también contiene ensamblados de otros proyectos y cualquier ensamblado de código abierto o de terceros que esté usando, y estos deben estar en el servidor de producción. Por lo tanto, como regla general, copie la carpeta `Bin` en producción al implementar. (Si usa el modelo de compilación automática y no usa ningún ensamblado externo, no tendrá un directorio `Bin`, ya que es correcto).

| **Modelo de compilación** | **¿Implementar el archivo de parte de marcado?** | **¿Implementar el archivo de código fuente?** | **¿Implementar ensamblados en `Bin` directorio?** |
| --- | --- | --- | --- |
| Compilación explícita | Sí | No | Sí |
| Compilación automática | Sí | Sí | Sí (si existe) |

**Tabla 1:** Los archivos que implemente dependen del modelo de compilación utilizado.

## <a name="taking-a-trip-down-memory-lane"></a>Poner un recorrido de la memoria hacia abajo

El enfoque de compilación que se usa depende, en parte, de cómo se administra la aplicación ASP.NET en Visual Studio. Desde. Inicio de la red en el año 2000 hay cuatro versiones diferentes de Visual Studio: Visual Studio .NET 2002, Visual Studio .NET 2003, Visual Studio 2005 y Visual Studio 2008. Visual Studio .NET 2002 y 2003 ASP.NET aplicaciones administradas mediante el *modelo de proyecto de aplicación web*. Las principales características del modelo de proyecto de aplicación web son:

- Los archivos que estructuran el proyecto se definen en un único archivo de proyecto. Los archivos no definidos en el archivo de proyecto no se consideran parte de la aplicación Web mediante Visual Studio.
- Utiliza la compilación explícita. Al compilar el proyecto, se compilan los archivos de código del proyecto en un ensamblado único que se coloca en la carpeta `Bin`.

Cuando Microsoft lanzó Visual Studio 2005, quitaba la compatibilidad con el modelo de proyecto de aplicación web y la reemplazaba por el modelo de proyecto de sitio Web. El modelo de proyecto de sitio web se ha diferenciado del modelo de proyecto de aplicación Web de las siguientes maneras:

- En lugar de tener un único archivo de proyecto que deletrea los archivos del proyecto, se usa el sistema de archivos en su lugar. En Resumen, los archivos dentro de la carpeta de la aplicación web (o subcarpetas) se consideran parte del proyecto.
- Al compilar un proyecto en Visual Studio, no se crea un ensamblado en el directorio de `Bin`. En su lugar, la creación de un proyecto de sitio web informa de los errores en tiempo de compilación.
- Compatibilidad con la compilación automática. Los proyectos de sitio web normalmente se implementan copiando el marcado y el código fuente en el entorno de producción, aunque el código puede estar precompilado (compilación explícita).

Microsoft reviviron el modelo de proyecto de aplicación web cuando lanzó Visual Studio 2005 Service Pack 1. Sin embargo, Visual Web Developer siguió admitiendo solo el modelo de proyecto de sitio Web. La buena noticia es que esta limitación se ha quitado con Visual Web Developer 2008 Service Pack 1. Hoy en día, puede crear aplicaciones de ASP.NET en Visual Studio (y Visual Web Developer) mediante el modelo de proyecto de aplicación web o el modelo de proyecto de sitio Web. Ambos modelos tienen sus ventajas y desventajas. Consulte [Introducción a los proyectos de aplicación web: comparación de proyectos de sitios web y proyectos de aplicación web](https://msdn.microsoft.com/library/aa730880.aspx#wapp_topic5) para obtener una comparación de los dos modelos y para ayudar a decidir qué modelo de proyecto funciona mejor para su situación.

## <a name="exploring-the-sample-web-application"></a>Exploración de la aplicación Web de ejemplo

La descarga de este tutorial incluye una aplicación ASP.NET denominada Book reviews. El sitio web imita a un sitio web de afición que podría crear para compartir sus opiniones de libros con la comunidad en línea. Esta aplicación Web de ASP.NET es muy sencilla y consta de los siguientes recursos:

- `Web.config`, el archivo de configuración de la aplicación.
- Página maestra (`Site.master`).
- Siete páginas de ASP.NET diferentes: 

    - ~`/Default.aspx`: la Página principal del sitio.
    - ~`/About.aspx`: página "acerca del sitio".
    - ~`/Fiction/Default.aspx`: una página en la que se muestran los libros de ficción que se han revisado. 

        - ~`/Fiction/Blaze.aspx`: una revisión de Richard Bachman novela *Blaze*.
    - ~/`Tech/Default.aspx`: una página en la que se muestran los libros de tecnología que se han revisado. 

        - ~/`Tech/CYOW.aspx`: una revisión de *crear su propio sitio web*.
        - ~/`Tech/TYASP35.aspx`: una revisión de *ASP.NET 3,5 en 24 horas*.
- Tres archivos CSS diferentes en la carpeta Styles.
- Cuatro archivos de imagen: un logotipo con tecnología de ASP.NET e imágenes de las cubiertas de los tres libros revisados, todo ello en la carpeta `Images`.
- Un archivo de `Web.sitemap`, que define el mapa del sitio y se usa para mostrar menús en las páginas de `Default.aspx` del directorio raíz y `Fiction` y `Tech` carpetas.
- Un archivo de clase denominado `BasePage.cs` que define una clase base `Page`. Esta clase extiende la funcionalidad de la clase `Page` estableciendo automáticamente la propiedad `Title` basándose en la posición de la página en el mapa del sitio. En pocas palabras, cualquier clase de código subyacente de ASP.NET que extienda `BasePage` (en lugar de `System.Web.UI.Page`) tendrá su título establecido en un valor en función de su posición en el mapa del sitio. Por ejemplo, al ver la página ~/`Tech/CYOW.aspx`, el título se establece en "Inicio: tecnología: crear su propio sitio web".

En la figura 1 se muestra una captura de pantalla del sitio web de revisiones del libro cuando se ve a través de un explorador. Aquí verá la página ~/`Tech/TYASP35.aspx`, que revisará el libro *ASP.NET 3,5 en 24 horas*. La ruta de navegación que abarca la parte superior de la página y el menú de la columna izquierda se basan en la estructura del mapa del sitio definida en `Web.sitemap`. La imagen de la esquina superior derecha es una de las imágenes de la cubierta del libro que se encuentra en la carpeta `Images`. La apariencia del sitio web se define a través de reglas de hoja de estilos en cascada escritas por los archivos CSS en la carpeta de estilos, mientras que el diseño de página más alto se define en la página maestra, `Site.master`.

[![el sitio web de revisiones del libro ofrece revisiones en una serie de títulos](determining-what-files-need-to-be-deployed-cs/_static/image2.png)](determining-what-files-need-to-be-deployed-cs/_static/image1.png)

**Figura 1:** El sitio web de revisiones del libro ofrece revisiones sobre una serie de títulos ([haga clic para ver la imagen de tamaño completo](determining-what-files-need-to-be-deployed-cs/_static/image3.png))

Esta aplicación no utiliza una base de datos; cada revisión se implementa como una página web independiente en la aplicación. Este tutorial (y los siguientes tutoriales) le guían a través de la implementación de una aplicación web que no tiene una base de datos. Sin embargo, en un futuro tutorial mejoraremos esta aplicación para almacenar revisiones, comentarios de lector y otra información en una base de datos, y exploraremos qué pasos se deben realizar para implementar correctamente una aplicación web controlada por datos.

> [!NOTE]
> Estos tutoriales se centran en el hospedaje de aplicaciones de ASP.NET con un proveedor de host web y no exploran temas accesorios como ASP. Sistema del mapa del sitio de la red o mediante una clase base `Page`. Para obtener más información sobre estas tecnologías, y para obtener más información sobre otros temas tratados en el tutorial, consulte la sección lecturas adicionales al final de cada tutorial.

La descarga de este tutorial tiene dos copias de la aplicación Web, cada una de ellas implementada como un tipo de proyecto de Visual Studio diferente: BookReviewsWAP, un proyecto de aplicación web y BookReviewsWSP, un proyecto de sitio Web. Ambos proyectos se crearon con Visual Web Developer 2008 SP1 y usan ASP.NET 3,5 SP1. Para trabajar con estos proyectos, empiece por descomprimir el contenido en el escritorio. Para abrir el proyecto de aplicación web (BookReviewsWAP), vaya a la carpeta BookReviewsWAP y haga doble clic en el archivo de solución `BookReviewsWAP.sln`. Para abrir el proyecto de sitio web (BookReviewsWSP), inicie Visual Studio y, a continuación, en el menú Archivo, elija la opción abrir sitio web, vaya a la carpeta `BookReviewsWSP` del escritorio y haga clic en Aceptar.

Las dos secciones restantes de este tutorial examinan qué archivos necesitará copiar en el entorno de producción al implementar la aplicación. Los dos tutoriales siguientes: *[implementar el sitio mediante FTP](deploying-your-site-using-an-ftp-client-cs.md)* e *[implementar el sitio con Visual Studio](deploying-your-site-using-visual-studio-cs.md)* : mostrar diferentes maneras de copiar estos archivos en un proveedor de hosts Web.

## <a name="determining-the-files-to-deploy-for-the-web-application-project"></a>Determinar los archivos que se van a implementar para el proyecto de aplicación Web

El modelo de proyecto de aplicación Web usa la compilación explícita: el código fuente del proyecto se compila en un único ensamblado cada vez que se compila la aplicación. Esta compilación incluye los archivos de código subyacente de las páginas ASP.NET (~/`Default.aspx.cs`, ~/`About.aspx.cs`, etc.), así como la clase `BasePage.cs`. El ensamblado resultante se denomina BookReviewsWAP. dll y se encuentra en el directorio de `Bin` de la aplicación.

En la figura 2 se muestran los archivos que componen el proyecto de aplicación Web de reseñas de libros.

[![el Explorador de soluciones enumera los archivos que componen el proyecto de aplicación Web](determining-what-files-need-to-be-deployed-cs/_static/image5.png)](determining-what-files-need-to-be-deployed-cs/_static/image4.png)

**Figura 2**: en el explorador de soluciones se enumeran los archivos que componen el proyecto de aplicación Web.

Para implementar una aplicación de ASP.NET desarrollada con el modelo de proyecto de aplicación Web, empiece por compilar la aplicación para compilar explícitamente el código fuente más reciente en un ensamblado. A continuación, copie los siguientes archivos en el entorno de producción:

- Archivos que contienen el marcado declarativo para cada página de ASP.NET, como ~/`Default.aspx`, ~/`About.aspx`, etc. Además, copie el marcado declarativo de las páginas maestras y los controles de usuario.
- Los ensamblados (archivos`.dll`) de la carpeta `Bin`. No es necesario copiar los archivos de base de datos de programa (`.pdb`) ni los archivos XML que encuentre en el directorio `Bin`.

No es necesario copiar los archivos de código fuente de ASP.NET pages en el entorno de producción ni copiar el archivo de clase `BasePage.cs`.

> [!NOTE]
> Como se muestra en la figura 2, la clase `BasePage` se implementa como un archivo de clase en el proyecto, colocado en la carpeta denominada `HelperClasses`. Cuando se compila el proyecto, el código del archivo `BasePage.cs` se compila junto con las clases de código subyacente de las páginas de ASP.NET en el ensamblado único, `BookReviewsWAP.dll.` ASP.NET tiene una carpeta especial denominada `App_Code` que está diseñada para contener archivos de clase para proyectos de sitios Web. El código de la carpeta `App_Code` se compila automáticamente y, por tanto, no se debe usar con proyectos de aplicación Web. En su lugar, debe colocar los archivos de clase de la aplicación en una carpeta normal denominada `HelperClasses`, o `Classes`o algo similar. Como alternativa, puede colocar archivos de clase en un proyecto de biblioteca de clases independiente.

Además de copiar los archivos de marcado relacionados con ASP.NET y el ensamblado en la carpeta `Bin`, también debe copiar los archivos de compatibilidad del lado cliente (las imágenes y los archivos CSS), así como los demás archivos de compatibilidad del lado servidor, `Web.config` y `Web.sitemap`. Estos archivos de compatibilidad del lado cliente y el servidor deben copiarse en el entorno de producción, independientemente de si se usa la compilación automática o explícita.

## <a name="determining-the-files-to-deploy-for-the-web-site-project-files"></a>Determinar los archivos que se van a implementar para los archivos de proyecto de sitio web

El modelo de proyecto de sitio web admite la compilación automática, una característica que no está disponible cuando se usa el modelo de proyecto de aplicación Web. Con la compilación explícita, debe compilar el código fuente del proyecto en un ensamblado y copiar ese ensamblado en el entorno de producción. Por otro lado, con la compilación automática simplemente se copia el código fuente en el entorno de producción y se compila mediante el tiempo de ejecución a petición según sea necesario.

La opción de menú compilar de Visual Studio está presente en los proyectos de aplicación web y en los proyectos de sitio Web. Al compilar proyectos de aplicación Web, el código fuente del proyecto se compila en un único ensamblado situado en el directorio `Bin`. al compilar un proyecto de sitio web, se comprueban los errores en tiempo de compilación, pero no se crea ningún ensamblado. Para implementar una aplicación de ASP.NET desarrollada con el modelo de proyecto de sitio web, lo único que debe hacer es copiar los archivos adecuados en el entorno de producción, pero le recomendamos que compile primero el proyecto para asegurarse de que no hay ningún error en tiempo de compilación.

En la figura 3 se muestran los archivos que componen el proyecto de sitio web de reseñas de libros.

 [![el Explorador de soluciones enumera los archivos que componen el proyecto de sitio web](determining-what-files-need-to-be-deployed-cs/_static/image7.png)](determining-what-files-need-to-be-deployed-cs/_static/image6.png) 

**Figura 3**: en el explorador de soluciones se enumeran los archivos que componen el proyecto de sitio web

La implementación de un proyecto de sitio web implica copiar todos los archivos relacionados con ASP.NET en el entorno de producción, que incluye las páginas de marcado para páginas de ASP.NET, páginas maestras y controles de usuario, junto con sus archivos de código. También debe copiar los archivos de clase, como BasePage.cs. Tenga en cuenta que el archivo de `BasePage.cs` se encuentra en la carpeta `App_Code`, que es una carpeta especial de ASP.NET que se usa en los proyectos de sitio web para los archivos de clase. También es necesario crear la carpeta especial en producción, ya que los archivos de clase de la carpeta `App_Code` del entorno de desarrollo deben copiarse en la carpeta `App_Code` en producción.

Además de copiar el marcado ASP.NET y los archivos de código fuente, también debe copiar los archivos de compatibilidad del lado cliente (las imágenes y los archivos CSS), así como los demás archivos de compatibilidad del lado servidor, `Web.config` y `Web.sitemap`.

> [!NOTE]
> Los proyectos de sitio web también pueden utilizar la compilación explícita. En un futuro tutorial se examinará cómo compilar explícitamente un proyecto de sitio Web.

## <a name="summary"></a>Resumen

La implementación de una aplicación ASP.NET implica copiar los archivos necesarios del entorno de desarrollo en el entorno de producción. El conjunto preciso de archivos que se deben sincronizar depende de si el código de la aplicación ASP.NET se compila explícita o automáticamente. La estrategia de compilación empleada se ve afectada por si Visual Studio está configurado para administrar la aplicación ASP.NET con el modelo de proyecto de aplicación web o el modelo de proyecto de sitio Web.

El modelo de proyecto de aplicación web utiliza la compilación explícita y compila el código del proyecto en un ensamblado único en la carpeta `Bin`. Al implementar la aplicación, se debe insertar en el entorno de producción la parte de marcado de las páginas ASP.NET y el contenido de la carpeta `Bin`. el código fuente de la aplicación: no es necesario copiar los archivos de código y las clases de código subyacente, por ejemplo, en el entorno de producción.

El modelo de proyecto de sitio web utiliza la compilación automática de forma predeterminada, aunque es posible compilar explícitamente un proyecto de sitio web, como veremos en futuros tutoriales. La implementación de una aplicación de ASP.NET que usa la compilación automática requiere que la parte de marcado *y* el código fuente se copien en el entorno de producción. El código se compila automáticamente en el entorno de producción cuando se solicita por primera vez.

Ahora que hemos examinado qué archivos se deben sincronizar entre los entornos de desarrollo y producción, estamos preparados para implementar la aplicación de reseñas de libros en un proveedor de host Web.

¡ Feliz programación!

### <a name="further-reading"></a>Información adicional

Para obtener más información sobre los temas descritos en este tutorial, consulte los siguientes recursos:

- [Información general de la compilación de ASP.NET](https://msdn.microsoft.com/library/ms178466.aspx)
- [Controles de usuario de ASP.NET](https://msdn.microsoft.com/library/y6wb1a0e.aspx)
- [Examinando ASP. Navegación del sitio de la red](http://aspnet.4guysfromrolla.com/articles/111605-1.aspx)
- [Introducción a los proyectos de aplicación Web](https://msdn.microsoft.com/library/aa730880.aspx)
- [Tutoriales de página maestra](../master-pages/creating-a-site-wide-layout-using-master-pages-cs.md)
- [Compartir código entre páginas](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/pages/code.aspx)
- [Usar una clase base personalizada para las clases de código subyacente de las páginas de ASP.NET](http://aspnet.4guysfromrolla.com/articles/041305-1.aspx)
- [Sistema del proyecto de sitio web de Visual Studio 2005: ¿Qué es y por qué lo hacemos?](https://weblogs.asp.net/scottgu/archive/2005/08/21/423201.aspx)
- [Tutorial: convertir un proyecto de sitio web en un proyecto de aplicación web en Visual Studio](https://msdn.microsoft.com/library/aa983476.aspx)

> [!div class="step-by-step"]
> [Anterior](asp-net-hosting-options-cs.md)
> [Siguiente](deploying-your-site-using-an-ftp-client-cs.md)
