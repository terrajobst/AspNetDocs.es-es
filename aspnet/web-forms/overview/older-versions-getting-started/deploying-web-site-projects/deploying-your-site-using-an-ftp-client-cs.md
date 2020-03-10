---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/deploying-your-site-using-an-ftp-client-cs
title: Implementación del sitio mediante un cliente FTP (C#) | Microsoft Docs
author: rick-anderson
description: La manera más sencilla de implementar una aplicación de ASP.NET consiste en copiar manualmente los archivos necesarios del entorno de desarrollo en el entorno de producción. ...
ms.author: riande
ms.date: 04/01/2009
ms.assetid: a3599cf7-8474-4006-954a-3bc693736b66
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/deploying-your-site-using-an-ftp-client-cs
msc.type: authoredcontent
ms.openlocfilehash: a3474650939ee220b3fd712e9f5a6cf3db11db09
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78438949"
---
# <a name="deploying-your-site-using-an-ftp-client-c"></a>Implementar el sitio mediante un cliente FTP (C#)

por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descargar código](https://download.microsoft.com/download/4/5/F/45F815EC-8B0E-46D3-9FB8-2DC015CCA306/ASPNET_Hosting_Tutorial_03_CS.zip) o [Descargar PDF](https://download.microsoft.com/download/E/8/9/E8920AE6-D441-41A7-8A77-9EF8FF970D8B/aspnet_tutorial03_DeployingViaFTP_cs.pdf)

> La manera más sencilla de implementar una aplicación de ASP.NET consiste en copiar manualmente los archivos necesarios del entorno de desarrollo en el entorno de producción. En este tutorial se muestra cómo usar un cliente FTP para obtener los archivos del escritorio en el proveedor de host Web.

## <a name="introduction"></a>Introducción

En el tutorial anterior se presentó una sencilla aplicación Web ASP.NET de revisión de libro, que se compone de una serie de páginas ASP.NET, una página maestra, una clase base `Page` personalizada, un número de imágenes y tres hojas de estilos CSS. Ahora estamos listos para implementar esta aplicación en un proveedor de hosts Web, momento en el que la aplicación estará accesible para cualquier persona con una conexión a Internet.

En las conversaciones del tutorial [*determinación de los archivos que se deben implementar*](determining-what-files-need-to-be-deployed-cs.md) , sabemos qué archivos deben copiarse en el proveedor de host Web. (Recuerde que los archivos que se copian dependen de si la aplicación se compila explícita o automáticamente). Pero, ¿cómo obtenemos los archivos del entorno de desarrollo (nuestro escritorio) hasta el entorno de producción (el servidor web administrado por el proveedor de host Web)? El archivo de [ **F** . **T** ransfer **P** rotocol (FTP)](http://en.wikipedia.org/wiki/File_Transfer_Protocol) es un protocolo de uso común para copiar archivos de un equipo a otro a través de una red. Otra opción es Extensiones de servidor de FrontPage (FPSE). Este tutorial se centra en el uso del software cliente FTP independiente para implementar los archivos necesarios desde el entorno de desarrollo en el entorno de producción.

> [!NOTE]
> Visual Studio incluye herramientas para publicar sitios web a través de FTP. Estas herramientas, así como un vistazo a las herramientas que usan FPSE, se describen en el siguiente tutorial.

Para copiar los archivos mediante FTP, se necesita un *cliente FTP* en el entorno de desarrollo. Un cliente FTP es una aplicación que está diseñada para copiar archivos del equipo en que se instala en un equipo que ejecuta un *servidor FTP*. (Si el proveedor de host Web admite transferencias de archivos a través de FTP, como la mayoría de ellos, entonces hay un servidor FTP que se ejecuta en sus servidores Web). Hay una serie de aplicaciones cliente FTP disponibles. El explorador Web puede incluso doblar como cliente FTP. Mi cliente FTP favorito, y el que usaremos para este tutorial, es [FileZilla](http://filezilla-project.org/), un cliente FTP gratuito y de código abierto que está disponible para Windows, Linux y Mac. Sin embargo, cualquier cliente FTP funcionará, por lo que puede usar cualquier cliente con el que esté más familiarizado.

Si sigue adelante, tendrá que crear una cuenta con un proveedor de hosts web para poder completar este tutorial o los siguientes. Como se indicó en el tutorial anterior, hay un Gaggle de compañías de proveedores de hosts web con una amplia gama de precios, características y calidad de servicio. En esta serie de tutoriales, usaremos el [descuento ASP.net](http://discountasp.net) como proveedor de host Web, pero puede seguir con cualquier proveedor de host web siempre que admitan la versión de ASP.net en la que se desarrolla su sitio. (Estos tutoriales se crearon con ASP.NET 3,5). Además, dado que se copiarán los archivos en el proveedor de host Web mediante FTP en este tutorial y en los futuros es imperativo que el proveedor de host web admita el acceso FTP a sus servidores Web. Prácticamente todos los proveedores de host Web ofrecen esta característica, pero debe comprobar antes de registrarse.

## <a name="deploying-the-book-review-web-application-project"></a>Implementación del proyecto de aplicación web revisar libro

Recuerde que hay dos versiones de la aplicación web revisar libro: una implementada mediante el modelo de proyecto de aplicación web (BookReviewsWAP) y la otra mediante el modelo de proyecto de sitio web (BookReviewsWSP). El tipo de proyecto influye en si el sitio se compila de forma automática o explícita, y ese modelo de compilación determina qué archivos se deben implementar. Por lo tanto, examinaremos la implementación de los proyectos BookReviewsWAP y BookReviewsWSP por separado, empezando por el BookReviewsWAP. Dedique un momento a descargar estas dos aplicaciones de ASP.NET si todavía no lo ha hecho.

Inicie el proyecto BookReviewsWAP; para ello, vaya a la carpeta `BookReviewsWAP` y haga doble clic en el archivo `BookReviewsWAP.sln`. Antes de implementar el proyecto es importante compilarlo para asegurarse de que los cambios realizados en el código fuente se incluyen en el ensamblado compilado. Para compilar el proyecto, vaya al menú compilar y elija la opción de menú compilar BookReviewsWAP. Compila el código fuente del proyecto en un único ensamblado, `BookReviewsWAP.dll`, que se coloca en la carpeta `Bin`.

Ahora estamos listos para implementar los archivos necesarios. Inicie el cliente FTP y conéctese al servidor Web en el proveedor de host Web. (Al registrarse en una empresa de hospedaje Web, se le enviará información sobre cómo conectarse al servidor FTP; esto incluye la dirección del servidor FTP, así como un nombre de usuario y una contraseña).

Copie los siguientes archivos del escritorio en la carpeta del sitio Web raíz en el proveedor de host Web. Cuando se hospeda en FTP en el servidor Web en el proveedor de host Web, es probable que se encuentre en el directorio del sitio Web raíz. Sin embargo, algunos proveedores de host web tienen una subcarpeta denominada `www` o `wwwroot` que actúa como carpeta raíz para los archivos del sitio Web. Por último, cuando FTPing los archivos, puede que necesite crear la estructura de carpetas correspondiente en el entorno de producción: la carpeta `Bin`, la carpeta `Fiction`, la carpeta `Images`, etc.

- `~/Default.aspx`
- `~/About.aspx`
- `~/Site.master`
- `~/Web.config`
- `~/Web.sitemap`
- El contenido completo de la carpeta `Styles`
- El contenido completo de la carpeta `Images` (y su subcarpeta, `BookCovers`)
- `~/Fiction/Default.aspx`
- `~/Fiction/Blaze.aspx`
- `~/Tech/Default.aspx`
- `~/Tech/CYOW.aspx`
- `~/Tech/TYASP35.aspx`
- `~/Bin/BookReviewsWAP.dll`

La figura 1 muestra FileZilla una vez copiados los archivos necesarios. FileZilla muestra los archivos en el equipo local a la izquierda y los archivos del equipo remoto a la derecha. Como se muestra en la figura 1, los archivos de código fuente de ASP.NET, como `About.aspx.cs`, se encuentran en el equipo local (el entorno de desarrollo), pero no se copiaron en el proveedor de host web (el entorno de producción) porque no es necesario implementar los archivos de código cuando se usa la compilación explícita.

> [!NOTE]
> No hay ningún daño en tener los archivos de código fuente en el servidor de producción, ya que se omiten. ASP.NET prohíbe las solicitudes HTTP a archivos de código fuente de forma predeterminada, de modo que, aunque los archivos de código fuente estén presentes en el servidor de producción, no estén accesibles para los visitantes de su sitio Web. (Es decir, si un usuario intenta visitar `http://www.yoursite.com/Default.aspx.cs` recibirá una página de error que explica que estos tipos de archivos `.cs` archivos están prohibidos).

[![usar un cliente FTP para copiar los archivos necesarios desde el escritorio al servidor Web en el proveedor de hosts Web](deploying-your-site-using-an-ftp-client-cs/_static/image2.png)](deploying-your-site-using-an-ftp-client-cs/_static/image1.png)

**Figura 1**: uso de un cliente FTP para copiar los archivos necesarios del escritorio en el servidor Web en el proveedor de host web ([haga clic para ver la imagen de tamaño completo](deploying-your-site-using-an-ftp-client-cs/_static/image3.png))

Después de implementar el sitio, dedique un momento a probarlo. Si ha adquirido un nombre de dominio y ha configurado correctamente la configuración de DNS, puede visitar su sitio escribiendo su nombre de dominio. Como alternativa, el proveedor de host Web debe haber proporcionado una dirección URL a su sitio, que tendrá un aspecto similar a *AccountName*. *webhostprovider*. com o *webhostprovider*. com/*AccountName*. Por ejemplo, la dirección URL de mi cuenta en Discount ASP.NET es: `http://httpruntime.web703.discountasp.net`.

En la ilustración 2 se muestra el sitio de revisiones del libro implementado. Tenga en cuenta que veo el descuento de ASP. Servidores de la red, en `http://httpruntime.web703.discountasp.net`. En este momento, cualquier persona con una conexión a Internet podría ver mi sitio Web. Como cabría esperar, el sitio parece y se comporta del mismo modo que cuando lo probó en el entorno de desarrollo.

> [!NOTE]
> Si recibe un error al ver la aplicación, dedique un momento a asegurarse de que ha implementado el conjunto de archivos correcto. A continuación, compruebe el mensaje de error para ver si revela alguna pista sobre el problema. Después, puede pasar al Departamento de soporte técnico de su empresa host o publicar su pregunta en el foro correspondiente en los [foros de ASP.net](https://forums.asp.net/).

[![el sitio de reseñas de libros ahora es accesible para cualquier persona con conexión a Internet](deploying-your-site-using-an-ftp-client-cs/_static/image5.png)](deploying-your-site-using-an-ftp-client-cs/_static/image4.png)

**Figura 2**: el sitio de reseñas de libros ahora es accesible para cualquier persona con una conexión a Internet ([haga clic para ver la imagen a tamaño completo](deploying-your-site-using-an-ftp-client-cs/_static/image6.png))

## <a name="deploying-the-book-review-web-site-project"></a>Implementación del proyecto de sitio web revisar libro

Al implementar una aplicación de ASP.NET que usa la compilación automática, como el proyecto de sitio web de BookReviewsWSP, no hay ningún ensamblado compilado en la carpeta `Bin`. Como resultado, los archivos de código fuente de la aplicación web se deben implementar en el entorno de producción. Vamos a examinar este proceso.

Al igual que con el proyecto de aplicación Web, es recomendable compilar primero la aplicación antes de implementarla. Al compilar un proyecto de sitio web no se crea un ensamblado, se comprueban los errores en tiempo de compilación de la página. Mejor encontrar estos errores ahora en lugar de tener un visitante en el sitio que los detecte.

Una vez que haya compilado correctamente el proyecto, use el cliente FTP para copiar los siguientes archivos en la carpeta del sitio Web raíz en el proveedor de host Web. Es posible que tenga que crear la estructura de carpetas correspondiente en el entorno de producción.

> [!NOTE]
> Si ya ha implementado el proyecto BookReviewsWAP pero desea intentar implementar el proyecto de BookReviewsWSP, primero elimine todos los archivos del servidor Web que se cargaron al implementar BookReviewsWAP y, a continuación, implemente los archivos para BookReviewsWSP.

- `~/Default.aspx`
- `~/Default.aspx.cs`
- `~/About.aspx`
- `~/About.aspx.cs`
- `~/Site.master`
- `~/Site.master.cs`
- `~/Web.config`
- `~/Web.sitemap`
- El contenido completo de la carpeta `Styles`
- El contenido completo de la carpeta `Images` (y su subcarpeta, `BookCovers`)
- `~/App_Code/BasePage.cs`
- `~/Fiction/Default.aspx`
- `~/Fiction/Default.aspx.cs`
- `~/Fiction/Blaze.aspx`
- `~/Fiction/Blaze.aspx.cs`
- `~/Tech/Default.aspx`
- `~/Tech/Default.aspx.cs`
- `~/Tech/CYOW.aspx`
- `~/Tech/CYOW.aspx.cs`
- `~/Tech/TYASP35.aspx`
- `~/Tech/TYASP35.aspx.cs`

En la figura 3 se muestra FileZilla después de copiar los archivos necesarios. Como puede ver, los archivos de código fuente de ASP.NET, como `About.aspx.cs`, están presentes en el equipo local (el entorno de desarrollo) y en el proveedor de host web (el entorno de producción), ya que los archivos de código deben implementarse cuando se usa la compilación automática.

[![usar un cliente FTP para copiar los archivos necesarios desde el escritorio al servidor Web en el proveedor de hosts Web](deploying-your-site-using-an-ftp-client-cs/_static/image8.png)](deploying-your-site-using-an-ftp-client-cs/_static/image7.png)

**Figura 3**: uso de un cliente FTP para copiar los archivos necesarios del escritorio en el servidor Web en el proveedor de host web ([haga clic para ver la imagen de tamaño completo](deploying-your-site-using-an-ftp-client-cs/_static/image9.png))

La experiencia del usuario no se ve afectada por el modelo de compilación de la aplicación. Se puede tener acceso a las mismas páginas de ASP.NET y tienen el mismo aspecto y comportamiento si el sitio web se creó utilizando el modelo de proyecto de aplicación web o el modelo de proyecto de sitio Web.

### <a name="updating-a-web-application-on-production"></a>Actualización de una aplicación web en producción

El desarrollo y la implementación de aplicaciones web no son un proceso único. Por ejemplo, al crear el sitio web de revisión de libros, creé las distintas páginas y escribía el código que lo acompaña en mi equipo personal (el entorno de desarrollo). Después de alcanzar un estado estable determinado, implementé mi aplicación para que otros usuarios pudieran visitar el sitio y leer mis opiniones. Pero la implementación no marca el final del desarrollo en este sitio. Puedo agregar más reseñas del libro o implementar nuevas características, como permitir que mis visitantes califiquen los libros o dejen sus comentarios. Estas mejoras se desarrollarán en el entorno de desarrollo y, cuando se complete, tendrían que implementarse. Por lo tanto, el desarrollo y la implementación son cíclicos. Desarrolle una aplicación y, a continuación, impleméntela. Mientras el sitio está activo y en producción, se agregan nuevas características y los errores se corrigen con el tiempo, lo que requiere volver a implementar la aplicación. Etc., etc.

Como cabría esperar, al volver a implementar una aplicación Web, solo tiene que copiar los archivos nuevos y modificados. No es necesario volver a implementar páginas sin cambios o archivos de compatibilidad del lado cliente o servidor (aunque no haya ningún daño al hacerlo).

> [!NOTE]
> Lo que hay que tener en cuenta al usar la compilación explícita es que cada vez que se agrega una nueva página de ASP.NET al proyecto o se realizan cambios relacionados con el código, es necesario volver a compilar el proyecto, lo que actualiza el ensamblado en la carpeta `Bin`. Por lo tanto, debe copiar este ensamblado actualizado en producción al actualizar una aplicación web en producción (junto con el otro contenido nuevo y actualizado).

Comprenda también que cualquier cambio en la `Web.config` o los archivos del directorio de `Bin` se detiene y reinicia el grupo de aplicaciones del sitio Web. Si el estado de sesión se almacena mediante el modo de `InProc` (valor predeterminado), los visitantes del sitio perderán su estado de sesión cada vez que se modifiquen estos archivos de claves. Para evitar este problema, considere la posibilidad de almacenar la sesión mediante los modos `StateServer` o `SQLServer`. Para obtener más información sobre este tema [, lea modos de estado de sesión](https://msdn.microsoft.com/library/ms178586.aspx).

Por último, tenga en cuenta que volver a implementar una aplicación puede tardar desde unos segundos hasta varios minutos, en función del número y el tamaño de los archivos que se deben copiar en el entorno de producción. Durante este tiempo, los usuarios que visitan su sitio pueden experimentar errores o un comportamiento extraño. Puede "desactivar" toda la aplicación agregando una página denominada `App_Offline.htm` al directorio raíz de la aplicación que explica a los usuarios que el sitio está inactivo por mantenimiento (o el que sea) y que se incluirán en breve. Cuando el archivo `App_Offline.htm` está presente, el tiempo de ejecución de ASP.NET redirige todas las solicitudes entrantes a esa página.

## <a name="summary"></a>Resumen

La implementación de una aplicación web implica copiar los archivos necesarios del entorno de desarrollo en el entorno de producción. El medio más común por el que los archivos se transfieren a través de una red es el File Transfer Protocol (FTP), y la mayoría de los proveedores de host web admiten el acceso FTP a sus servidores Web. En este tutorial vimos cómo usar un cliente FTP para implementar los archivos necesarios en el servidor Web. Una vez implementado, cualquier persona con una conexión a Internet puede visitar el sitio Web.

¡ Feliz programación!

### <a name="further-reading"></a>Información adicional

Para obtener más información sobre los temas descritos en este tutorial, consulte los siguientes recursos:

- [La aplicación\_. htm sin conexión y está trabajando en torno a la característica "errores descriptivos de IE"](https://weblogs.asp.net/scottgu/App_5F00_Offline.htm-and-working-around-the-_2200_IE-Friendly-Errors_2200_-feature)
- [Modos de estado de sesión](https://msdn.microsoft.com/library/ms178586.aspx)

> [!div class="step-by-step"]
> [Anterior](determining-what-files-need-to-be-deployed-cs.md)
> [Siguiente](deploying-your-site-using-visual-studio-cs.md)
