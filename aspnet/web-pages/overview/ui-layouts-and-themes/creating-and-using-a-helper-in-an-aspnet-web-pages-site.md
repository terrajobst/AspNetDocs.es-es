---
uid: web-pages/overview/ui-layouts-and-themes/creating-and-using-a-helper-in-an-aspnet-web-pages-site
title: Crear y usar una aplicación auxiliar en un sitio de ASP.NET Web Pages (Razor) | Microsoft Docs
author: Rick-Anderson
description: En este artículo se describe cómo crear una aplicación auxiliar en un sitio web de ASP.NET Web Pages (Razor). Una aplicación auxiliar es un componente reutilizable que incluye código y marcado en perf...
ms.author: riande
ms.date: 02/17/2014
ms.assetid: 46bff772-01e0-40f0-9ae6-9e18c5442ee6
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/creating-and-using-a-helper-in-an-aspnet-web-pages-site
msc.type: authoredcontent
ms.openlocfilehash: 380663951094c9fc7d5f0601e30995fa073a204b
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78454309"
---
# <a name="creating-and-using-a-helper-in-an-aspnet-web-pages-razor-site"></a>Crear y usar una aplicación auxiliar en un sitio de ASP.NET Web Pages (Razor)

por [Tom FitzMacken](https://github.com/tfitzmac)

> En este artículo se describe cómo crear una aplicación auxiliar en un sitio web de ASP.NET Web Pages (Razor). Una *aplicación auxiliar* es un componente reutilizable que incluye código y marcado para realizar una tarea que podría ser tediosa o compleja.
> 
> **Lo que aprenderá:** 
> 
> - Cómo crear y usar una aplicación auxiliar simple.
> 
> Estas son las características de ASP.NET presentadas en el artículo:
> 
> - Sintaxis de `@helper`.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versiones de software usadas en el tutorial
> 
> 
> - ASP.NET Web Pages (Razor) 3
>   
> 
> Este tutorial también funciona con ASP.NET Web Pages 2.

## <a name="overview-of-helpers"></a>Información general de las aplicaciones auxiliares

Si necesita realizar las mismas tareas en diferentes páginas de su sitio, puede usar una aplicación auxiliar. ASP.NET Web Pages incluye una serie de aplicaciones auxiliares y hay muchas más que puede descargar e instalar. (Una lista de las aplicaciones auxiliares integradas en ASP.NET Web Pages se muestra en la [referencia rápida](https://go.microsoft.com/fwlink/?LinkId=202907)de la API de ASP.net). Si ninguna de las aplicaciones auxiliares existentes satisface sus necesidades, puede crear su propia aplicación auxiliar.

Una aplicación auxiliar le permite usar un bloque de código común en varias páginas. Supongamos que, en la página, a menudo desea crear un elemento de nota que se diferencia de los párrafos normales. Quizás la nota se crea como un elemento `<div>` con el estilo de cuadro con un borde. En lugar de agregar este mismo marcado a una página cada vez que desea mostrar una nota, puede empaquetar el marcado como una aplicación auxiliar. Después, puede insertar la nota con una sola línea de código en cualquier lugar en que la necesite.

El uso de una aplicación auxiliar como esta hace que el código de cada una de las páginas sea más sencillo y fácil de leer. También facilita el mantenimiento del sitio, porque si necesita cambiar el aspecto de las notas, puede cambiar el marcado en un solo lugar.

## <a name="creating-a-helper"></a>Crear una aplicación auxiliar

En este procedimiento se muestra cómo crear la aplicación auxiliar que crea la nota, tal y como se describe aquí. Este es un ejemplo sencillo, pero el ayudante personalizado puede incluir cualquier marcado y el código de ASP.NET que necesite.

1. En la carpeta raíz del sitio web, cree una carpeta denominada *App\_Code*. Se trata de un nombre de carpeta reservado en ASP.NET donde puede colocar código para componentes como aplicaciones auxiliares.
2. En la carpeta de *código del\_* de la aplicación, cree un nuevo archivo *. cshtml* y asígnele el nombre mis *aplicaciones. cshtml*.
3. Reemplace el contenido existente por lo siguiente:

    [!code-cshtml[Main](creating-and-using-a-helper-in-an-aspnet-web-pages-site/samples/sample1.cshtml)]

    El código usa la sintaxis de `@helper` para declarar un nuevo ayudante denominado `MakeNote`. Esta aplicación auxiliar concreta le permite pasar un parámetro denominado `content` que puede contener una combinación de texto y marcado. La aplicación auxiliar inserta la cadena en el cuerpo de la nota mediante el `@content` variable.

    Tenga en cuenta que el nombre del archivo es me *helper. cshtml*, pero el ayudante se denomina `MakeNote`. Puede incluir varias aplicaciones auxiliares personalizadas en un único archivo.
4. Guarde y cierre el archivo.

## <a name="using-the-helper-in-a-page"></a>Usar el ayudante en una página

1. En la carpeta raíz, cree un nuevo archivo en blanco denominado *TestHelper. cshtml*.
2. Agregue el código siguiente al archivo:

    [!code-html[Main](creating-and-using-a-helper-in-an-aspnet-web-pages-site/samples/sample2.html)]

    Para llamar al ayudante que creó, utilice `@` seguido del nombre de archivo donde el ayudante es, un punto y, a continuación, el nombre de la aplicación auxiliar. (Si tiene varias carpetas en la carpeta de *código del\_* de la aplicación, puede usar la sintaxis `@FolderName.FileName.HelperName` para llamar a la aplicación auxiliar en cualquier nivel de carpeta anidada). El texto que agregue entre comillas entre paréntesis es el texto que el ayudante mostrará como parte de la nota en la Página Web.
3. Guarde la página y ejecútela en un explorador. El ayudante genera el elemento de nota justo donde llamó a la aplicación auxiliar: entre los dos párrafos.

    ![Captura de pantalla que muestra la página en el explorador y cómo el ayudante generó el marcado que coloca un cuadro alrededor del texto especificado.](creating-and-using-a-helper-in-an-aspnet-web-pages-site/_static/image1.png)

## <a name="additional-resources"></a>Recursos adicionales

[Menú horizontal como aplicación auxiliar de Razor](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2341). En esta entrada de blog de Mike Pope se muestra cómo crear un menú horizontal como ayudante mediante marcado, CSS y código.

Uso de [HTML5 en ASP.NET Web pages aplicaciones auxiliares para WebMatrix y ASP.net MVC3](http://geekswithblogs.net/wildturtle/archive/2010/11/08/html5-in-asp.net-web-pages-helpers-for-webmatrix-and_aspnet_mvc3.aspx). En esta entrada de blog de Sam Abraham se muestra una aplicación auxiliar que representa un elemento de `Canvas` HTML5.

[La diferencia entre @Helpers y @Functions en WebMatrix](http://www.mikesdotnetting.com/Article/173/The-Difference-Between-@Helpers-and-@Functions-In-WebMatrix). En esta entrada de blog, Mike Salmuered describe `@helper` sintaxis y `@function` sintaxis y Cuándo usar cada una.
