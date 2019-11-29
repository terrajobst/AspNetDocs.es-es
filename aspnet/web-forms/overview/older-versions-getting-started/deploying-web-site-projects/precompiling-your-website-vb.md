---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/precompiling-your-website-vb
title: Precompilar el sitio web (VB) | Microsoft Docs
author: rick-anderson
description: 'Visual Studio ofrece a los desarrolladores de ASP.NET dos tipos de proyectos: proyectos de aplicación web (WAP) y proyectos de sitio web (WSPs). Una de las diferencias clave Betwe...'
ms.author: riande
ms.date: 06/09/2009
ms.assetid: c285dc6f-a1c6-46e6-ac03-3830947f57e3
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/precompiling-your-website-vb
msc.type: authoredcontent
ms.openlocfilehash: 1a46870b73f95300ef5bc1f72dda74d7d62ab11f
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/28/2019
ms.locfileid: "74588317"
---
# <a name="precompiling-your-website-vb"></a>Precompilar el sitio web (VB)

por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descargar código](https://download.microsoft.com/download/1/0/C/10CC829F-A808-4302-97D3-59989B8F9C01/ASPNET_Hosting_Tutorial_15_VB.zip) o [Descargar PDF](https://download.microsoft.com/download/5/C/5/5C57DB8C-5DEA-4B3A-92CA-4405544D313B/aspnet_tutorial15_Precompiling_vb.pdf)

> Visual Studio ofrece a los desarrolladores de ASP.NET dos tipos de proyectos: proyectos de aplicación web (WAP) y proyectos de sitio web (WSPs). Una de las diferencias principales entre los dos tipos de proyecto es que WAP debe tener el código compilado explícitamente antes de la implementación, mientras que el código de un WSP se puede compilar automáticamente en el servidor Web. Sin embargo, es posible precompilar un WSP antes de la implementación. En este tutorial se exploran las ventajas de la precompilación y se muestra cómo precompilar un sitio web desde Visual Studio y desde la línea de comandos.

## <a name="introduction"></a>Introducción

Visual Studio ofrece a los desarrolladores de ASP.NET dos tipos de proyecto diferentes: proyectos de aplicación web (WAP) y proyectos de sitio web (WSP). Una de las diferencias principales entre estos tipos de proyecto es que WAP requiere *compilación explícita* , mientras que WSPs usa la *compilación automática*, de forma predeterminada. Con WAP, el código de la aplicación web se compila en un único ensamblado, que se crea en la carpeta `Bin` del sitio Web. La implementación implica copiar el contenido de marcado (los archivos `.aspx.ascx`y `.master`) en el proyecto, junto con el ensamblado en la carpeta `Bin`. no es necesario implementar los propios archivos de clase de código subyacente. Por otro lado, implemente WSPs copiando las páginas de marcado y sus clases de código subyacente correspondientes en el entorno de producción. Las clases de código subyacente se compilan a petición en el servidor Web.

> [!NOTE]
> Consulte la sección "compilación explícita frente a compilación automática" en el tutorial [ *determinación de los archivos que se deben implementar* ](determining-what-files-need-to-be-deployed-vb.md) para obtener más información sobre las diferencias entre los modelos de proyecto, la compilación explícita y automática, y cómo afecta el modelo de compilación a la implementación.

La opción de compilación automática es fácil de usar. No hay ningún paso de compilación explícito y solo es necesario implementar los archivos que se han modificado, mientras que la compilación explícita requiere la implementación de las páginas de marcado modificadas y el ensamblado de compilación Just-in. Sin embargo, la implementación automática tiene dos desventajas potenciales:

- Dado que las páginas se deben compilar automáticamente cuando se visitan por primera vez, puede haber un retraso breve pero perceptible cuando se solicita una página de ASP.NET por primera vez después de implementarse.
- La compilación automática requiere que tanto el marcado declarativo como el código fuente estén presentes en el servidor Web. Esta opción puede ser poco atractiva si planea vender la aplicación web a los clientes que la instalarán en sus servidores Web.

Si cualquiera de las dos deficiencias anteriores son los disyuntores, puede cambiar al modelo WAP o *precompilar* el WSP antes de la implementación. En este tutorial se examinan las opciones de precompilación más adecuadas para un sitio web hospedado y se explica el proceso de precompilación y la implementación de un sitio web precompilado.

## <a name="an-overview-of-aspnet-code-generation-and-compilation"></a>Información general sobre la generación y compilación de código ASP.NET

Antes de examinar las opciones de precompilación disponibles, vamos a hablar de la generación y compilación de código que se produce cuando se solicita por primera vez una página ASP.NET desde que se creó o actualizó por última vez. Como sabe, las páginas de ASP.NET se componen de dos partes: marcado declarativo en el archivo `.aspx`; y una parte de código fuente, normalmente en un archivo de clase de código subyacente (`.aspx.vb`) independiente. Los pasos realizados por el motor en tiempo de ejecución cuando se solicita una página ASP.NET dependen del modelo de compilación de la aplicación.

Con WAP, el código fuente de las páginas se debe compilar explícitamente en un único ensamblado antes de implementarse. Durante la implementación, este ensamblado y las distintas páginas de marcado se copian en el entorno de producción. Cuando llega una solicitud al servidor Web de una página de ASP.NET, el tiempo de ejecución crea una instancia de la clase de código subyacente de la página e invoca su método `ProcessRequest`, que inicia el ciclo de vida de la página y, en última instancia, genera el contenido de la página, que se devuelve al solicitante. El runtime puede trabajar con la clase de código subyacente de la página ASP.NET porque la clase de código subyacente ya estaba compilada en un ensamblado antes de la implementación.

Con WSPs y la compilación automática, no hay ningún paso explícito de compilación antes de la implementación. En su lugar, la implementación implica copiar el contenido del código de origen y el declarativo en el entorno de producción. Cuando una solicitud llega al servidor web para una página de ASP.NET por primera vez desde que se creó o actualizó por última vez la página, el tiempo de ejecución debe compilar primero la clase de código subyacente en un ensamblado. Este ensamblado compilado se guarda en la carpeta `%WINDIR%\Microsoft.NET\Framework\v2.0.50727\Temporary ASP.NET Files`, aunque la ubicación de esta carpeta se puede personalizar mediante el elemento `<pages>` de `Web.config`. Dado que el ensamblado se guarda en el disco, no es necesario volver a compilar en las solicitudes posteriores a la misma página.

> [!NOTE]
> Como cabría esperar, hay un ligero retraso al solicitar una página por primera vez (o por primera vez desde que se ha cambiado) en un sitio que usa la compilación automática, ya que el servidor tarda un momento en compilar el código de la página y guardar el ensamblado resultante en discos.

En Resumen, con la compilación explícita, es necesario compilar el código fuente del sitio Web antes de la implementación, lo que evita que el tiempo de ejecución tenga que realizar este paso. Con la compilación automática, el tiempo de ejecución controla la compilación del código fuente de las páginas, pero con un ligero costo de inicialización para la primera visita a la página desde que se creó o actualizó por última vez.

Pero ¿qué ocurre con la parte declarativa de las páginas de ASP.NET (el archivo `.aspx`)? Es obvio que hay una relación entre los archivos de `.aspx` y el código de las clases de código subyacente, ya que los controles Web definidos en el marcado declarativo son accesibles en el código. También es obvio que el contenido de los archivos `.aspx` influye en gran medida en el marcado representado generado por la página. Así pues, ¿cómo funciona el tiempo de ejecución con el texto, el código HTML y la sintaxis de control Web definidos en el archivo `.aspx` para generar el contenido representado de la página solicitada?

No quiero obtener demasiada Sidetracked en los detalles de implementación de bajo nivel, que varían entre WAP y WSPs, pero en pocas palabras el tiempo de ejecución genera automáticamente un archivo de clase que contiene los distintos controles Web como miembros y métodos protegidos. Este archivo generado se implementa como una *clase parcial* para la clase de código subyacente correspondiente. ([Las clases parciales](http://www.dotnet-guide.com/partialclasses.html) permiten repartir el contenido de una sola clase en varios archivos). Por lo tanto, la clase de código subyacente se define en dos lugares: en el `.aspx.vb` archivo que se crea y en esta clase generada automáticamente creada por el motor en tiempo de ejecución. Esta clase generada automáticamente se almacena en la carpeta `%WINDIR%\Microsoft.NET\Framework\v2.0.50727\Temporary ASP.NET Files`.

Lo más importante es que, para que la página de ASP.NET sea representada por el tiempo de ejecución, las partes declarativas y de código fuente deben compilarse en un ensamblado. Con WAP, el código fuente se compila explícitamente en un ensamblado antes de la implementación, pero el marcado declarativo todavía se debe convertir en código y compilarlo en el tiempo de ejecución en el servidor Web. Con WSPs mediante la compilación automática, el código fuente y el marcado declarativo deben ser compilados por el servidor Web.

Es posible usar la compilación explícita con el modelo WSP. Puede compilar explícitamente la parte del código fuente, como con el modelo WAP. Además, también puede compilar el marcado declarativo.

## <a name="precompilation-options"></a>Opciones de precompilación

El .NET Framework se suministra con una [herramienta de compilación ASP.net (`aspnet_compiler.exe`)](https://msdn.microsoft.com/library/ms229863.aspx) que le permite compilar el código fuente (e incluso el contenido) de una aplicación ASP.net compilada con el modelo WSP. Esta herramienta se lanzó con la .NET Framework versión 2,0 y se encuentra en la carpeta `%WINDIR%\Microsoft.NET\Framework\v2.0.50727`. se puede usar desde la línea de comandos o iniciar desde Visual Studio a través de la opción publicar sitio web del menú compilar.

La herramienta de compilación proporciona dos formas generales de compilación: precompilación en contexto y precompilación para la implementación. Con la precompilación en contexto, ejecute la herramienta de `aspnet_compiler.exe` desde la línea de comandos y especifique la ruta de acceso al directorio virtual o a la ruta de acceso física de un sitio web que resida en el equipo. A continuación, la herramienta de compilación compila cada página de ASP.NET en el proyecto, almacenando la versión compilada en la `%WINDIR%\Microsoft.NET\Framework\v2.0.50727\Temporary ASP.NET Files` carpeta como si las páginas se hubieran visitado por primera vez desde un explorador. La precompilación en contexto puede acelerar la primera solicitud realizada a las páginas de ASP.NET recién implementadas en el sitio, ya que evita que el tiempo de ejecución tenga que realizar este paso. Sin embargo, la precompilación en contexto no es útil para la mayoría de los sitios web hospedados porque requiere que se puedan ejecutar programas desde la línea de comandos del servidor Web. En entornos de hospedaje compartidos no se permite este nivel de acceso.

> [!NOTE]
> Para obtener más información sobre la precompilación en contexto, consulte [Cómo: precompilar sitios Web ASP.net](https://msdn.microsoft.com/library/ms227972.aspx) y [precompilación en ASP.net 2,0](http://www.odetocode.com/Articles/417.aspx).

En lugar de compilar las páginas del sitio web en la carpeta `Temporary ASP.NET Files`, la precompilación para la implementación compila las páginas en un directorio de su elección y en un formato que se puede implementar en el entorno de producción.

Hay dos tipos de precompilación para la implementación que se exploran en este tutorial: precompilación con una interfaz de usuario actualizable y precompilación con una interfaz de usuario no actualizable. La precompilación con una interfaz de usuario actualizable deja el marcado declarativo en los archivos `.aspx`, `.ascx`y `.master`, lo que permite al desarrollador ver y, si se desea, modificar el marcado declarativo en el servidor de producción. La precompilación con una interfaz de usuario no actualizable genera `.aspx` páginas que son nulas de cualquier contenido y quita `.ascx` y `.master` archivos, con lo que se oculta el marcado declarativo y se prohíbe a un desarrollador cambiarlo desde el entorno de producción.

### <a name="precompiling-for-deployment-with-an-updatable-user-interface"></a>Precompilación para la implementación con una interfaz de usuario actualizable

La mejor manera de comprender la precompilación para la implementación es ver un ejemplo en acción. Vamos a compilar el libro para la implementación mediante una interfaz de usuario actualizable. La herramienta de compilación ASP.NET se puede invocar desde el menú compilar de Visual Studio o desde la línea de comandos. En esta sección se examina el uso de la herramienta desde Visual Studio. en la sección "precompilación desde la línea de comandos" se examina la ejecución de la herramienta compilador desde la línea de comandos.

Abra el libro revisar WSP en Visual Studio, vaya al menú compilar y seleccione la opción de menú publicar sitio Web. Se abrirá el cuadro de diálogo Publicar sitio web (vea la **figura 1**), donde puede especificar la ubicación de destino, si la interfaz de usuario del sitio precompilado es actualizable y otras opciones de la herramienta de compilación. La ubicación de destino puede ser un servidor Web remoto o un servidor FTP, pero, por ahora, elija una carpeta en el disco duro del equipo. Dado que queremos precompilar el sitio con una interfaz de usuario actualizable, deje activada la casilla "permitir que este sitio precompilado se actualice" y haga clic en Aceptar.

[![](precompiling-your-website-vb/_static/image2.png)](precompiling-your-website-vb/_static/image1.png)

**Figura 1**: la herramienta de compilación de ASP.net precompilará el sitio web en la ubicación de destino especificada.  
 ([Haga clic para ver la imagen de tamaño completo](precompiling-your-website-vb/_static/image3.png))

> [!NOTE]
> La opción publicar sitio web del menú compilar no está disponible en Visual Web Developer. Si usa Visual Web Developer, tendrá que usar la versión de línea de comandos de la herramienta de compilación ASP.NET, que se describe en la sección "precompilar desde la línea de comandos".

Después de precompilar el sitio web, vaya a la ubicación de destino que especificó en el cuadro de diálogo Publicar sitio Web. Dedique un momento a comparar el contenido de esta carpeta con el contenido de su sitio Web. En la **ilustración 2** se muestra la carpeta del sitio web de reseñas de libros. Tenga en cuenta que contiene archivos `.aspx` y `.aspx.cs`. Además, tenga en cuenta que el directorio `Bin` incluye solo un archivo, `Elmah.dll`, que hemos agregado en el [tutorial anterior](logging-error-details-with-elmah-vb.md) .

[![](precompiling-your-website-vb/_static/image5.png)](precompiling-your-website-vb/_static/image4.png)

**Figura 2**: el directorio del proyecto contiene archivos de `.aspx` y `.aspx.cs`; la carpeta `Bin` incluye solo `Elmah.dll`  
 ([Haga clic para ver la imagen de tamaño completo](precompiling-your-website-vb/_static/image6.png))

En la **figura 3** se muestra la carpeta de ubicación de destino cuyo contenido se creó con la herramienta de compilación ASP.net. Esta carpeta no contiene ningún archivo de código subyacente. Además, el directorio `Bin` de esta carpeta incluye varios ensamblados y dos archivos `.compiled` además del ensamblado `Elmah.dll`.

[![](precompiling-your-website-vb/_static/image8.png)](precompiling-your-website-vb/_static/image7.png)

**Figura 3**: la carpeta de ubicación de destino incluye los archivos para la implementación  
 ([Haga clic para ver la imagen de tamaño completo](precompiling-your-website-vb/_static/image9.png))

A diferencia de la compilación explícita en WAP, la precompilación para el proceso de implementación no crea un ensamblado para todo el sitio. En su lugar, agrupa varias páginas en cada ensamblado. También compila el archivo `Global.asax` (si existe) en su propio ensamblado, así como cualquier clase de la carpeta `App_Code`. Los archivos que contienen el marcado declarativo para las páginas Web ASP.NET, los controles de usuario y las páginas maestras (`.aspx`, `.ascx`y archivos `.master`, respectivamente) se copian tal cual en el directorio de ubicación de destino. Del mismo modo, el archivo de `Web.config` se copia directamente, junto con cualquier archivo estático, como imágenes, clases CSS y archivos PDF. Para obtener una descripción más formal de cómo la herramienta de compilación controla varios tipos de archivo, consulte [control de archivos durante la precompilación de ASP.net](https://msdn.microsoft.com/library/e22s60h9.aspx).

> [!NOTE]
> Puede indicar a la herramienta de compilación que cree un ensamblado por cada página de ASP.NET, control de usuario o página maestra activando la casilla "se usaron nombres corregidos y ensamblados de una sola página" del cuadro de diálogo Publicar sitio Web. Cada página de ASP.NET compilada en su propio ensamblado permite un control más preciso sobre la implementación. Por ejemplo, si actualiza una única página web de ASP.NET y necesita implementar ese cambio, solo necesita implementar el archivo de `.aspx` de la página y el ensamblado asociado en el entorno de producción. Consulte [Cómo: generar nombres fijos con la herramienta de compilación ASP.net](https://msdn.microsoft.com/library/ms228040.aspx) para obtener más información.

El directorio de ubicación de destino también contiene un archivo que no formaba parte del proyecto web precompilado, es decir, `PrecompiledApp.config`. Este archivo informa al tiempo de ejecución de ASP.NET de que la aplicación estaba precompilada y de si se precompiló con una interfaz de usuario actualizable o de Noon.

Por último, dedique un momento a abrir uno de los archivos de `.aspx` en la ubicación de destino mediante Visual Studio o el editor de texto que prefiera. Al precompilar para la implementación con una interfaz de usuario actualizable, las páginas de ASP.NET en el directorio de ubicación de destino contienen exactamente el mismo marcado que los archivos correspondientes en el sitio Web.

### <a name="precompiling-for-deployment-with-a-non-updatable-user-interface"></a>Precompilación para la implementación con una interfaz de usuario no actualizable

La herramienta de compilador ASP.NET también se puede usar para precompilar un sitio para la implementación con una interfaz de usuario no actualizable. Precompilar el sitio con una interfaz de usuario que no se pueda actualizar funciona de manera muy similar a la precompilación con una interfaz de usuario actualizable, la diferencia clave es que las páginas ASP.NET, los controles de usuario y las páginas maestras del directorio de destino se quitan del marcado. Para precompilar un sitio web para la implementación con una interfaz de usuario no actualizable, elija la opción publicar sitio web en el menú compilar, pero desactive la opción "permitir que este sitio precompilado se actualice" (consulte la **figura 4**).

[![](precompiling-your-website-vb/_static/image11.png)](precompiling-your-website-vb/_static/image10.png)

**Figura 4**: Desactive la opción "permitir que este sitio precompilado se actualice" para precompilar con una interfaz de usuario no actualizable  
 ([Haga clic para ver la imagen de tamaño completo](precompiling-your-website-vb/_static/image12.png))

En la **figura 5** se muestra la carpeta de ubicación de destino después de precompilar con una interfaz de usuario no actualizable.

[![](precompiling-your-website-vb/_static/image14.png)](precompiling-your-website-vb/_static/image13.png)

**Figura 5**: la carpeta de ubicación de destino para la implementación con una interfaz de usuario no actualizable  
 ([Haga clic para ver la imagen de tamaño completo](precompiling-your-website-vb/_static/image15.png))

Compare la **figura 3** con la **figura 5**. Aunque las dos carpetas pueden tener un aspecto idéntico, tenga en cuenta que la carpeta de la interfaz de usuario no actualizable no tiene la página maestra, `Site.master`. Y mientras que la **figura 5** incluye las distintas páginas de ASP.net, si ve el contenido de estos archivos, verá que se han quitado su marcado declarativo y que se han reemplazado por el texto del marcador de posición: "este es un archivo de marcador generado por la herramienta de precompilación y no se debe eliminar".

[![](precompiling-your-website-vb/_static/image17.png)](precompiling-your-website-vb/_static/image16.png)

**Figura 5**: el marcado declarativo se ha quitado de las páginas de ASP.net

Las carpetas `Bin` en las **figuras 3** y **5** difieren de forma más sustancial. Además de los ensamblados, la carpeta `Bin` de la **figura 5** incluye un archivo `.compiled` para cada página de ASP.net, control de usuario y página maestra.

Precompilar un sitio con una interfaz de usuario no actualizable es útil en situaciones en las que no desea que el contenido de las páginas de ASP.NET sea modificado por la persona o la compañía que instala o administra el sitio web en el entorno de producción. Si crea una aplicación Web de ASP.NET que vende a los clientes para que la instalen en sus propios servidores Web, puede que desee asegurarse de que no modifican la apariencia y el funcionamiento de su sitio mediante la edición directa de las páginas `.aspx` que envía. Al precompilar el sitio web con una interfaz de usuario no actualizable, envía el marcador de posición `.aspx` páginas como parte de la instalación, lo que impide que los clientes examinen o modifiquen su contenido.

### <a name="precompiling-from-the-command-line"></a>Precompilar desde la línea de comandos

En segundo plano, el cuadro de diálogo Publicar sitio web de Visual Studio invoca la herramienta de compilación de ASP.NET (`aspnet_compiler.exe`) para precompilar el sitio Web. Como alternativa, puede invocar esta herramienta desde la línea de comandos. De hecho, si usa Visual Web Developer, tendrá que ejecutar la herramienta de compilación desde la línea de comandos, ya que el menú compilar de Visual Web Developer no incluye la opción publicar sitio Web.

Para usar la herramienta compilador desde la línea de comandos, empiece por colocar en la línea de comandos y navegue hasta el directorio del marco `%WINDIR%\Microsoft.NET\Framework\v2.0.50727`. A continuación, escriba la siguiente instrucción en la línea de comandos:

`aspnet_compiler -p "physical_path_to_app" -v / -f -u "target_location_folder"`

El comando anterior inicia la herramienta compilador de ASP.NET (`aspnet_compiler.exe`) y, a través del modificador `-p`, le indica que precompile el sitio Web raíz de la *ruta de acceso física\_\_a\_aplicación*; Este valor será similar a `C:\MySites\BookReviews`y debe delimitarse con comillas.

El modificador `-v` especifica el directorio virtual del sitio. Si el sitio está registrado como el sitio web predeterminado en la metabase de IIS, puede omitir el modificador `-p` y simplemente especificar el directorio virtual de la aplicación. Si usa el modificador `-p`, el valor que sigue al modificador `-v` indica la raíz del sitio web y se utiliza para resolver las referencias raíz de la aplicación. Por ejemplo, si especifica un valor de `-v /MySite`, las referencias de la aplicación a `~/path/file` se resolverán como `~/MySite/path/file`. Dado que el sitio de revisiones del libro se encuentra en el directorio raíz de mi empresa de hospedaje Web, se ha usado el modificador `-v /`.

El modificador `-f`, si está presente, indica a la herramienta de compilación que sobrescriba la *Ubicación del\_de destino\_* directorio de la carpeta si ya existe. Si omite el modificador `-f` y la carpeta de ubicación de destino ya existe, la herramienta de compilación se cerrará con el error: "error ASPRUNTIME: el directorio de destino no está vacío. Elimínelo manualmente o elija otro destino. "

El modificador `-u`, si está presente, informa a la herramienta para crear una interfaz de usuario actualizable. Omita este modificador para precompilar el sitio con una interfaz de usuario no actualizable.

Por último, el *destino\_ubicación\_carpeta* es la ruta de acceso física al directorio de ubicación de destino. Este valor será similar a `C:\MySites\Output\BookReviews`y debe delimitarse con comillas.

## <a name="deploying-the-precompiled-website"></a>Implementación del sitio web precompilado

En este punto, hemos aprendido a usar la herramienta de compilación de ASP.NET para precompilar un sitio web mediante las opciones de la interfaz de usuario actualizable y no actualizable. Sin embargo, los ejemplos han precompilado el sitio web en una carpeta local y no en el entorno de producción. La buena noticia es que la implementación del sitio web precompilado es muy sencilla y se puede realizar a través de Visual Studio o a través de algún otro mecanismo de copia de archivos, como desde un cliente FTP independiente.

El cuadro de diálogo Publicar sitio web (primero se muestra en la **figura 1**) tiene una opción ubicación de destino, que indica dónde se copian los archivos del sitio web precompilado. Esta ubicación puede ser un servidor Web remoto o un servidor FTP. Al escribir un servidor remoto en este cuadro de texto, se precompila e implementa el sitio web en el servidor especificado en un solo paso. Como alternativa, puede precompilar el sitio web en una carpeta local y, a continuación, copiar manualmente el contenido de la carpeta en el entorno de producción a través de FTP o cualquier otro enfoque.

Tener el sitio web precompilado implementado automáticamente mediante el cuadro de diálogo Publicar sitio web de Visual Studio resulta útil para sitios sencillos en los que no hay ninguna diferencia de configuración entre el entorno de desarrollo y el de producción. Sin embargo, como se indicó en las [ *diferencias de configuración comunes entre el tutorial de desarrollo y producción* ](common-configuration-differences-between-development-and-production-vb.md) , no es raro que existan diferencias. Por ejemplo, la aplicación Web de reseñas de libros utiliza una base de datos diferente en el entorno de producción que en el entorno de desarrollo. Cuando Visual Studio publica el sitio web en un servidor remoto, copia ciegamente la información del archivo de configuración en el entorno de desarrollo.

En el caso de los sitios con diferencias de configuración entre los entornos de desarrollo y producción, es posible que sea mejor precompilar el sitio en un directorio local, copiar los archivos de configuración específicos de producción y copiar el contenido de la salida precompilada en funcionamiento.

Para un actualizador al copiar archivos del entorno de desarrollo en el entorno de producción, consulte el tutorial [*implementación de su sitio web mediante un cliente FTP*](deploying-your-site-using-an-ftp-client-vb.md) e [*implementación de su sitio web mediante Visual Studio*](determining-what-files-need-to-be-deployed-vb.md) .

## <a name="summary"></a>Resumen

ASP.NET admite dos modos de compilación: automático y explícito. Como se explicó en los tutoriales anteriores, los proyectos de aplicación web (WAP) usan la compilación explícita, mientras que los proyectos de sitio web (WSPs) utilizan la compilación automática de forma predeterminada. Sin embargo, es posible compilar explícitamente un WSP antes de la implementación mediante la herramienta de compilación ASP.NET.

Este tutorial se centra en la precompilación de la herramienta de compilación para la compatibilidad con la implementación. Al precompilar para la implementación, la herramienta de compilación crea una carpeta de ubicación de destino, compila el código fuente de la aplicación Web especificada y copia estos ensamblados compilados y los archivos de contenido en la carpeta de ubicación de destino. La herramienta de compilación se puede configurar para crear una interfaz de usuario actualizable o no actualizable. Al precompilar con una opción de interfaz de usuario que no se puede actualizar, se quita el marcado declarativo en los archivos de contenido. En pocas palabras, la precompilación le permite implementar su aplicación basada en proyectos de sitio web sin incluir ningún archivo de código fuente y con el marcado declarativo quitado, si lo desea.

¡ Feliz programación!

### <a name="further-reading"></a>Información adicional

Para obtener más información sobre los temas descritos en este tutorial, consulte los siguientes recursos:

- [Precompilación de sitio web de ASP.NET](https://msdn.microsoft.com/library/ms228015.aspx)
- [Codebehind y compilación en ASP.NET 2,0](https://msdn.microsoft.com/magazine/cc163675.aspx)
- [Precompilación en ASP.NET](http://www.odetocode.com/Articles/417.aspx)
- [Opciones del sitio precompilado en ASP.NET](http://www.dotnetperls.com/precompiled)

> [!div class="step-by-step"]
> [Anterior](logging-error-details-with-elmah-vb.md)
> [Siguiente](users-and-roles-on-the-production-website-vb.md)
