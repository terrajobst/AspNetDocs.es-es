---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/deploying-your-site-using-visual-studio-cs
title: Implementación del sitio con Visual Studio (C#) | Microsoft Docs
author: rick-anderson
description: Visual Studio incluye herramientas para implementar un sitio Web. Obtenga más información sobre estas herramientas en este tutorial.
ms.author: riande
ms.date: 04/01/2009
ms.assetid: cde4ee53-a5d0-4937-a54b-67877e8266c3
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/deploying-your-site-using-visual-studio-cs
msc.type: authoredcontent
ms.openlocfilehash: 4259e51f5a3e6a97bae2aa27b76cbd56ca3449d6
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/28/2019
ms.locfileid: "74636357"
---
# <a name="deploying-your-site-using-visual-studio-c"></a>Implementar el sitio con Visual Studio (C#)

por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descargar código](https://download.microsoft.com/download/4/5/F/45F815EC-8B0E-46D3-9FB8-2DC015CCA306/ASPNET_Hosting_Tutorial_04_CS.zip) o [Descargar PDF](https://download.microsoft.com/download/E/8/9/E8920AE6-D441-41A7-8A77-9EF8FF970D8B/aspnet_tutorial04_DeployingViaVS_cs.pdf)

> Visual Studio incluye herramientas para implementar un sitio Web. Obtenga más información sobre estas herramientas en este tutorial.

## <a name="introduction"></a>Introducción

En el tutorial anterior se examinó cómo implementar una aplicación Web de ASP.NET sencilla en un proveedor de hosts Web. En concreto, en el tutorial se ha mostrado cómo usar un cliente FTP como FileZilla para transferir los archivos necesarios del entorno de desarrollo al entorno de producción. Visual Studio también ofrece herramientas integradas para facilitar la implementación en un proveedor de hosts Web. En este tutorial se examinan dos de estas herramientas: la herramienta Copiar sitio web, donde puede migrar archivos hacia y desde un servidor Web remoto mediante FTP o Extensiones de servidor de FrontPage; y la herramienta de publicación, que copia todo el sitio web en una ubicación especificada.

> [!NOTE]
> Otras herramientas relacionadas con la implementación que ofrece Visual Studio incluyen [proyectos de instalación web](https://msdn.microsoft.com/library/wx3b589t.aspx) y complementos de [proyectos de implementación web](https://www.microsoft.com/downloads/details.aspx?FamilyId=0AA30AE8-C73B-4BDD-BB1B-FE697256C459&amp;displaylang=en) . Los proyectos de instalación web empaquetan el contenido y la información de configuración de un sitio web en un único archivo MSI. Esta opción es muy útil para los sitios web que se implementan en una intranet o para empresas que venden una aplicación web preconfigurada que los clientes instalan en sus propios servidores Web. El complemento proyectos de implementación web es un complemento de Visual Studio que facilita la especificación de las diferencias de configuración entre las compilaciones para entornos de desarrollo y entornos de producción. Los proyectos de instalación web no se tratan en esta serie de tutoriales; Los proyectos de implementación web se resumen en el tutorial de [*diferencias de configuración comunes entre desarrollo y producción*](common-configuration-differences-between-development-and-production-cs.md) .

## <a name="deploying-your-site-using-the-copy-web-site-tool"></a>Implementar el sitio mediante la herramienta Copiar sitio web

La herramienta Copiar sitio web de Visual Studio tiene una funcionalidad similar a la de un cliente FTP independiente. En pocas palabras, la herramienta Copiar sitio web le permite conectarse a un sitio Web remoto a través de FTP o Extensiones de servidor de FrontPage. De forma similar a la interfaz de usuario de FileZilla, la interfaz de usuario de copiar sitio web consta de dos paneles: el panel izquierdo muestra los archivos locales mientras que el panel de la derecha muestra los archivos en el servidor de destino.

> [!NOTE]
> La herramienta Copiar sitio web solo está disponible para los proyectos de sitio Web. Visual Studio ofrece esta herramienta cuando se trabaja con un proyecto de aplicación Web.

Echemos un vistazo al uso de la herramienta Copiar sitio web para publicar la aplicación de revisión del libro en producción. Dado que la herramienta Copiar sitio web solo funciona con proyectos que utilizan el modelo de proyecto de sitio web, solo se puede examinar mediante esta herramienta con el proyecto BookReviewsWSP. Abra el proyecto.

Inicie el proyecto de herramienta Copiar sitio web haciendo clic en el icono Copiar sitio web del Explorador de soluciones (este icono aparece rodeado de un círculo en la figura 1). como alternativa, puede seleccionar la opción Copiar sitio web en el menú del sitio Web. Cualquier enfoque inicia la interfaz de usuario copiar sitio web que se muestra en la figura 1; solo se rellena el panel izquierdo de la figura 1 porque aún no se ha conectado a un servidor remoto.

[![la interfaz de usuario de la herramienta Copiar sitio web está dividida en dos paneles](deploying-your-site-using-visual-studio-cs/_static/image2.png)](deploying-your-site-using-visual-studio-cs/_static/image1.png)

**Figura 1**: la interfaz de usuario de la herramienta Copiar sitio web está dividida en dos paneles ([haga clic para ver la imagen de tamaño completo](deploying-your-site-using-visual-studio-cs/_static/image3.png))

Para implementar nuestro sitio, es necesario conectarse primero al proveedor de host Web. Haga clic en el botón conectar situado en la parte superior de la interfaz de usuario de copiar sitio Web. Esto muestra el cuadro de diálogo abrir sitio web que se muestra en la figura 2.

Puede conectarse al sitio web de destino seleccionando una de las cuatro opciones de la izquierda:

- **Sistema de archivos** : Seleccione esta acción para implementar el sitio en una carpeta o recurso compartido de red accesible desde el equipo.
- **IIS local** : Utilice esta opción para implementar el sitio en el servidor Web de IIS instalado en el equipo.
- **Sitio FTP** : conectarse a un sitio Web remoto mediante FTP.
- **Sitio remoto** : Conéctese a un sitio Web remoto mediante extensiones de servidor de FrontPage.

La mayoría de los proveedores de host web admiten FTP, pero menos compatibilidad con extensiones de servidor de FrontPage. Por ese motivo, seleccioné la opción sitio FTP y, a continuación, escribió la información de conexión como se muestra en la figura 2.

[![especificar el sitio web de destino](deploying-your-site-using-visual-studio-cs/_static/image5.png)](deploying-your-site-using-visual-studio-cs/_static/image4.png)

**Figura 2**: especificar el sitio web de destino ([haga clic para ver la imagen de tamaño completo](deploying-your-site-using-visual-studio-cs/_static/image6.png))

Después de conectarse, la herramienta Copiar sitio web carga los archivos en el sitio remoto en el panel derecho e indica el estado de cada archivo: nuevo, eliminado, cambiado o sin cambios. Puede copiar un archivo desde el sitio local en el sitio remoto, o viceversa.

Vamos a agregar una nueva página al proyecto BookReviewsWSP y, a continuación, implementarla para que podamos ver la herramienta Copiar sitio web en acción. Cree una nueva página de ASP.NET en Visual Studio en el directorio raíz denominado `Privacy.aspx`. Haga que la página use la página maestra `Site.master` y agregue la Directiva de privacidad del sitio a esta página. En la figura 3 se muestra Visual Studio después de crear esta página.

[![agregar una nueva página llamada &lt;Code&gt;privacy. aspx&lt;/Code&gt; a la carpeta raíz del sitio web](deploying-your-site-using-visual-studio-cs/_static/image8.png)](deploying-your-site-using-visual-studio-cs/_static/image7.png)

**Figura 3**: agregue una nueva página denominada `Privacy.aspx` a la carpeta raíz del sitio web ([haga clic para ver la imagen de tamaño completo](deploying-your-site-using-visual-studio-cs/_static/image9.png))

A continuación, vuelva a la interfaz de usuario de copiar sitio Web. Como se muestra en la figura 4, el panel izquierdo incluye ahora los nuevos archivos: `Policy.aspx` y `Policy.aspx.cs`. Además, estos archivos se marcan con un icono de flecha y un estado de nuevo, lo que indica que existen en el sitio local, pero no en el sitio remoto.

[![la herramienta Copiar sitio web incluye la nueva página código de &lt;&gt;privacy. aspx&lt;/Code&gt; en el panel izquierdo](deploying-your-site-using-visual-studio-cs/_static/image11.png)](deploying-your-site-using-visual-studio-cs/_static/image10.png)

**Figura 4**: la herramienta Copiar sitio web incluye la nueva página de `Privacy.aspx` en el panel izquierdo ([haga clic para ver la imagen a tamaño completo](deploying-your-site-using-visual-studio-cs/_static/image12.png))

Para implementar los nuevos archivos, selecciónelos y, a continuación, haga clic en el icono de flecha para transferirlos al sitio remoto. Una vez completada la transferencia, los archivos de `Policy.aspx` y `Policy.aspx.cs` existen tanto en el sitio local como en el remoto con el estado sin cambios.

Junto con la lista de nuevos archivos, la herramienta Copiar sitio web resalta los archivos que difieren entre los sitios locales y remotos. Para ver esto en acción, vuelva a la página `Privacy.aspx` y agregue algunas palabras a la Directiva de privacidad. Guarde la página y, a continuación, vuelva a la herramienta Copiar sitio Web. Como se muestra en la figura 5, la página `Privacy.aspx` del panel izquierdo tiene el estado cambiado, lo que indica que no está sincronizada con el sitio remoto.

[![la herramienta Copiar sitio web indica que se ha cambiado el código de &lt;&gt;privacy. aspx&lt;página de&gt;.](deploying-your-site-using-visual-studio-cs/_static/image14.png)](deploying-your-site-using-visual-studio-cs/_static/image13.png)

**Figura 5**: la herramienta Copiar sitio web indica que se ha cambiado la página `Privacy.aspx` ([haga clic para ver la imagen de tamaño completo](deploying-your-site-using-visual-studio-cs/_static/image15.png))

La herramienta Copiar sitio web también indica si un archivo se ha eliminado desde la última operación de copia. Elimine el `Privacy.aspx` del proyecto local y actualice la herramienta Copiar sitio Web. Los archivos `Privacy.aspx` y `Privacy.aspx.cs` permanecen en la lista del panel izquierdo, pero tienen un estado eliminado que indica que se han quitado desde la última operación de copia.

## <a name="publishing-a-web-application"></a>Publicación de una aplicación Web

Otra manera de implementar la aplicación web desde dentro de Visual Studio es usar la opción publicar, que es accesible a través del menú compilar. La opción publicar compila explícitamente la aplicación y, a continuación, copia todos los archivos necesarios hasta el sitio remoto especificado. Como veremos en breve, la opción publicar es más desigual que la herramienta Copiar sitio Web. Mientras que la herramienta Copiar sitio web le permite examinar los archivos en los sitios locales y remotos, y permite cargar o descargar archivos individuales según sea necesario, la opción publicar implementa toda la aplicación Web.

Además de copiar todos los archivos necesarios en el sitio remoto especificado, la opción de publicación también compila explícitamente la aplicación. Dado que los proyectos de aplicación web deben compilarse explícitamente, debería suponer que la opción publicar está disponible para los proyectos de aplicación Web. Lo que puede ser un poco sorprendente es que la opción publicar también está disponible para los proyectos de sitio Web. Como se indicó en el tutorial [*determinar qué archivos se deben implementar*](determining-what-files-need-to-be-deployed-cs.md) , los proyectos de sitios web se pueden compilar explícitamente a través de un proceso denominado *precompilación*. Este tutorial se centra en el uso de la opción de publicación con proyectos de aplicación Web. en el futuro, se explorará la precompilación, momento en que se devolverá el uso de la opción publicar con proyectos de sitio Web.

> [!NOTE]
> Aunque la opción publicar está disponible en Visual Studio para los proyectos de sitio web y los proyectos de aplicación Web, Visual Web Developer solo ofrece la opción publicar para proyectos de aplicación Web.

Echemos un vistazo a la implementación de la aplicación de revisiones del libro con la opción publicar. Para empezar, abra BookReviewsWAP (el proyecto de aplicación web) en Visual Studio. En el menú publicar, elija el proyecto compilar BookReviewsWAP. Aparecerá un cuadro de diálogo que solicita la ubicación de destino, entre otras opciones de configuración (vea la figura 6). Al igual que con la herramienta Copiar sitio web, puede escribir una ubicación que apunte a una carpeta local, un sitio Web local en IIS, un sitio Web remoto que admita Extensiones de servidor de FrontPage o una dirección de servidor FTP. Puede elegir si desea reemplazar los archivos del servidor Web remoto con los archivos implementados o eliminar todo el contenido del sitio remoto antes de publicarlo. También puede especificar si desea copiar:

- Solo los archivos del proyecto necesarios para ejecutar la aplicación, que omite el código fuente y los archivos relacionados con el proyecto innecesarios.
- Todos los archivos de proyecto, que incluyen los archivos de código fuente y los archivos de proyecto de Visual Studio, como el archivo de solución.
- Todos los archivos de la carpeta de proyecto de origen, que copia todos los archivos de la carpeta de proyecto de origen independientemente de si están incluidos en el proyecto.

También hay una opción para cargar el contenido de la carpeta `App_Data`.

[![especificar el sitio web de destino](deploying-your-site-using-visual-studio-cs/_static/image17.png)](deploying-your-site-using-visual-studio-cs/_static/image16.png)

**Figura 6**: especificar el sitio web de destino ([haga clic para ver la imagen de tamaño completo](deploying-your-site-using-visual-studio-cs/_static/image18.png))

En el libro revisar aplicación, el sitio remoto contiene los archivos implementados al copiar el proyecto BookReviewsWSP a través de la herramienta Copiar sitio Web. Por lo tanto, vamos a iniciar la opción de publicación eliminando todo el contenido existente. Además, vamos a copiar simplemente los archivos necesarios en lugar de saturar el entorno de producción con código fuente y archivos de proyecto innecesarios. Después de especificar estas opciones, haga clic en el botón publicar. En los próximos segundos, Visual Studio implementará los archivos necesarios en el sitio de destino, lo que mostrará su progreso en la ventana de salida.

La figura 7 muestra los archivos en el sitio FTP una vez completada la operación de publicación. Tenga en cuenta que solo se han cargado las páginas de marcado y los archivos de compatibilidad de cliente y de servidor necesarios.

[![solo se publicaron los archivos necesarios en el entorno de producción](deploying-your-site-using-visual-studio-cs/_static/image20.png)](deploying-your-site-using-visual-studio-cs/_static/image19.png)

**Figura 7**: solo se publicaron los archivos necesarios en el entorno de producción ([haga clic para ver la imagen de tamaño completo](deploying-your-site-using-visual-studio-cs/_static/image21.png))

La opción de publicación es una herramienta menos Matic que la herramienta Copiar sitio Web. Mientras que la herramienta Copiar sitio web permite inspeccionar los archivos en los sitios locales y remotos y ver cómo difieren, la opción publicar no proporciona ninguna interfaz de este tipo. Además, la herramienta Copiar sitio web permite realizar cambios desactivados, cargar o eliminar archivos individuales. La opción de publicación no permite el control específico. en su lugar, publica *toda* la aplicación. Este comportamiento tiene sus ventajas y desventajas. En el lado más, se sabe que al usar la opción de publicación no se puede olvidar cargar un archivo importante. Pero tenga en cuenta lo que sucede si ha realizado un pequeño cambio en un sitio web muy grande: con la opción de publicación no puede actualizar esa página o dos que se han modificado, pero en su lugar debe esperar mientras Visual Studio implementa todo el sitio.

No es raro que haya ciertos archivos cuyo contenido difiere entre los entornos de producción y desarrollo. Un ejemplo de clave es el archivo de configuración de la aplicación, `Web.config`. Dado que la opción de publicación copia ciegamente los archivos de aplicación Web, sobrescribe los archivos de configuración personalizados del entorno de producción con la versión del entorno de desarrollo. En el tutorial siguiente se explora este tema con más detalle y se ofrecen sugerencias para implementar una aplicación web cuando existen tales diferencias.

## <a name="summary"></a>Resumen

La implementación de un sitio web implica copiar los archivos necesarios del entorno de desarrollo en el entorno de producción. En el tutorial anterior se mostró cómo transferir archivos mediante un cliente FTP como FileZilla. En este tutorial se han examinado dos herramientas de implementación en Visual Studio: la herramienta Copiar sitio web y la opción publicar. La herramienta Copiar sitio web es similar a un cliente FTP en que tiene una interfaz de dos paneles en la que se muestran los archivos del equipo local y un equipo remoto especificado que facilita la carga o descarga de archivos entre los dos equipos. La opción publicar es una herramienta más despunta que compila explícitamente el proyecto y, a continuación, implementa toda la aplicación en el destino especificado.

¡ Feliz programación!

### <a name="further-reading"></a>Información adicional

Para obtener más información sobre los temas descritos en este tutorial, consulte los siguientes recursos:

- [Copiar el sitio web con la herramienta Copiar sitio web](https://msdn.microsoft.com/library/1cc82atw.aspx)
- [Cómo: implementar un sitio web mediante la herramienta Copiar sitio web](../../../videos/how-do-i/how-do-i-deploy-a-web-site-using-the-copy-web-site-tool.md) (vídeo)
- [Cómo: publicar proyectos de aplicación Web](https://msdn.microsoft.com/library/aa983453.aspx)
- [Cómo: publicar sitios web](https://msdn.microsoft.com/library/20yh9f1b.aspx)
- [Proyectos de instalación e implementación en Visual Studio](https://msdn.microsoft.com/library/wx3b589t.aspx)

> [!div class="step-by-step"]
> [Anterior](deploying-your-site-using-an-ftp-client-cs.md)
> [Siguiente](common-configuration-differences-between-development-and-production-cs.md)
