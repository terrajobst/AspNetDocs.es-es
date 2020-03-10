---
uid: web-pages/overview/ui-layouts-and-themes/10-working-with-video
title: Visualización de vídeo en un sitio de ASP.NET Web Pages (Razor) | Microsoft Docs
author: Rick-Anderson
description: En este capítulo se explica cómo mostrar vídeo en un ASP.NET Web Pages con sintaxis Razor página.
ms.author: riande
ms.date: 02/20/2014
ms.assetid: 332fb3da-e2a5-460d-bb90-dd911e1e2c95
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/10-working-with-video
msc.type: authoredcontent
ms.openlocfilehash: 516d46f38ce8910209f4207c474b0404bf012950
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78510385"
---
# <a name="displaying-video-in-an-aspnet-web-pages-razor-site"></a>Mostrar vídeo en un sitio de ASP.NET Web Pages (Razor)

por [Tom FitzMacken](https://github.com/tfitzmac)

> En este artículo se explica cómo usar un reproductor de vídeo (multimedia) en un sitio web de ASP.NET Web Pages (Razor) para permitir a los usuarios ver los vídeos almacenados en el sitio. ASP.NET Web Pages con sintaxis Razor le permite reproducir vídeos de Flash ( *. SWF*), Media Player ( *. wmv*) y Silverlight ( *. xap*).
> 
> Temas que se abordarán:
> 
> - Cómo elegir un reproductor de vídeo.
> - Cómo agregar vídeo a una página web.
> - Cómo establecer los atributos del reproductor de vídeo.
> 
> Estas son las características de las páginas de Razor ASP.NET presentadas en el artículo:
> 
> - Aplicación auxiliar de `Video`.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versiones de software usadas en el tutorial
> 
> 
> - ASP.NET Web Pages (Razor) 2
> - WebMatrix 2
>   
> 
> Este tutorial también funciona con WebMatrix 3.

## <a name="introduction"></a>Introducción

Es posible que desee mostrar un vídeo en el sitio. Una forma de hacerlo es vincular a un sitio que ya tiene el vídeo, como YouTube. Si desea insertar un vídeo desde estos sitios directamente en sus propias páginas, normalmente puede obtener el formato HTML del sitio y, a continuación, copiarlo en la página. Por ejemplo, en el ejemplo siguiente se muestra cómo insertar un vídeo de YouTube:

[!code-html[Main](10-working-with-video/samples/sample1.html?highlight=10-14)]

Si desea reproducir un vídeo que se encuentra en su propio sitio web (no en un sitio de uso compartido de vídeo público), no se puede vincular directamente a él mediante el marcado insertado como este. Sin embargo, puede reproducir vídeos desde su sitio mediante la aplicación auxiliar de `Video`, que representa un reproductor multimedia directamente en una página.

<a id="Choosing_a_Video_Player"></a>
## <a name="choosing-a-video-player"></a>Elección de un reproductor de vídeo

Hay muchos formatos para los archivos de vídeo y cada formato normalmente requiere un reproductor diferente y una manera diferente de configurar el reproductor. En las páginas de Razor ASP.NET, puede reproducir un vídeo en una página web con la aplicación auxiliar de `Video`. La aplicación auxiliar de `Video` simplifica el proceso de insertar vídeos en una página web, ya que genera automáticamente los elementos HTML `object` y `embed` que se usan normalmente para agregar vídeo a la página.

La aplicación auxiliar de `Video` admite los siguientes reproductores multimedia:

- Adobe Flash
- Windows MediaPlayer
- Microsoft Silverlight

### <a name="the-flash-player"></a>El reproductor de Flash

El reproductor de `Flash` de la aplicación auxiliar de `Video` le permite reproducir vídeos Flash (archivos *. SWF* ) en una página web. Como mínimo, debe proporcionar una ruta de acceso al archivo de vídeo. Si no especifica nada excepto la ruta de acceso, el reproductor usa los valores predeterminados establecidos por la versión actual de Flash. La configuración predeterminada típica es la siguiente:

- El vídeo se muestra con el ancho y el alto predeterminados y sin un color de fondo.
- El vídeo se reproduce automáticamente cuando se carga la página.
- El vídeo se repite continuamente hasta que se detiene explícitamente.
- El vídeo se escala para mostrar todo el vídeo, en lugar de recortar el vídeo para ajustarse a un tamaño específico.
- El vídeo se reproduce en una ventana.

### <a name="the-mediaplayer-player"></a>El reproductor de MediaPlayer

El reproductor de `MediaPlayer` de la aplicación auxiliar de `Video` le permite reproducir vídeos de Windows Media (archivos *. wmv* ), audio de Windows Media (archivos *. WMA* ) y MP3 (archivos *. mp3* ) en una página web. Debe incluir la ruta de acceso del archivo multimedia que se va a reproducir. todos los demás parámetros son opcionales. Si especifica solo una ruta de acceso, el reproductor utiliza la configuración predeterminada establecida por la versión actual de MediaPlayer, como:

- El vídeo se muestra con el ancho y el alto predeterminados.
- El vídeo se reproduce automáticamente cuando se carga la página.
- El vídeo se reproduce una vez (no se repite).
- El reproductor muestra el conjunto completo de controles en la interfaz de usuario.
- El vídeo se reproduce en una ventana.

### <a name="the-silverlight-player"></a>El reproductor de Silverlight

El reproductor de `Silverlight` de la aplicación auxiliar de `Video` le permite reproducir Windows Media Video (archivos *. wmv* ), Windows Media Audio (archivos *. WMA* ) y MP3 (archivos *. mp3* ). Debe establecer el parámetro Path para que señale a un paquete de aplicación basado en Silverlight (archivo *. xap* ). También debe establecer los parámetros de ancho y alto. Todos los demás parámetros son opcionales. Cuando se usa el reproductor de Silverlight para vídeo, si solo se establecen los parámetros necesarios, el reproductor de Silverlight muestra el vídeo sin un color de fondo.

> [!NOTE]
> En caso de que aún no conozca Silverlight: el archivo *. xap* es un archivo comprimido que contiene instrucciones de diseño en un archivo *. Xaml* , código administrado en ensamblados y recursos opcionales. Puede crear un archivo *. xap* en Visual Studio como un proyecto de aplicación de Silverlight.

El reproductor de vídeo `Silverlight` usa la configuración que se proporciona para el reproductor y la configuración que se proporciona en el archivo *. xap* .

> [!TIP] 
> 
> <a id="SB_MimeTypes"></a>
> ### <a name="mime-types"></a>Tipos de MIME
> 
> Cuando un explorador descarga un archivo, el explorador se asegura de que el tipo de archivo coincide con el tipo MIME especificado para el documento que se está representando. El tipo MIME es el tipo de contenido o el tipo de medio de un archivo. La aplicación auxiliar de `Video` usa los siguientes tipos MIME:
> 
> - `application/x-shockwave-flash`
> - `application/x-mplayer2`
> - `application/x-silverlight-2`

<a id="Playing_Flash"></a>
## <a name="playing-flash-swf-videos"></a>Reproducir vídeos Flash (. SWF)

En este procedimiento se muestra cómo reproducir un vídeo Flash denominado *sample. SWF*. En el procedimiento se supone que tiene una carpeta denominada *media* en el sitio y que el archivo *. SWF* está en esa carpeta.

1. Agregue la biblioteca de aplicaciones auxiliares Web de ASP.NET a su sitio web, tal como se describe en [instalación de aplicaciones auxiliares en un sitio de ASP.NET Web pages](https://go.microsoft.com/fwlink/?LinkId=252372), si aún no lo ha hecho.
2. En el sitio web, agregue una página y asígnele el nombre *FlashVideo. cshtml*.
3. Agregue el siguiente marcado a la página: 

    [!code-cshtml[Main](10-working-with-video/samples/sample2.cshtml)]
4. Ejecute la página en un explorador. (Asegúrese de que la página esté seleccionada en el área de trabajo **archivos** antes de ejecutarla). Se muestra la página y el vídeo se reproduce automáticamente. 

    ![impresión](10-working-with-video/_static/image1.jpg "ch08_video -1. jpg")

Puede establecer el parámetro `quality` para un vídeo Flash en `low`, `autolow`, `autohigh`, `medium`, `high`y `best`:

[!code-cshtml[Main](10-working-with-video/samples/sample3.cshtml)]

Puede cambiar el vídeo Flash para que se reproduzca con un tamaño específico mediante el parámetro `scale`, que puede establecer en lo siguiente:

- `showall`Operador Esto hace que todo el vídeo sea visible mientras mantiene la relación de aspecto original. Sin embargo, es posible que acabe con bordes en cada lado.
- `noorder`Operador Esto escala el vídeo mientras mantiene la relación de aspecto original, pero puede que se recorte.
- `exactfit`Operador Esto hace que todo el vídeo sea visible sin conservar la relación de aspecto original, pero puede producirse una distorsión.

Si no especifica un parámetro `scale`, todo el vídeo será visible y la relación de aspecto original se mantendrá sin recortes. En el ejemplo siguiente se muestra cómo usar el parámetro `scale`:

[!code-cshtml[Main](10-working-with-video/samples/sample4.cshtml)]

Flash Player admite una configuración de modo de vídeo denominada `windowMode`. Puede establecerlo en `window`, `opaque`y `transparent`. De forma predeterminada, la `windowMode` se establece en `window`, que muestra el vídeo en una ventana independiente de la Página Web. El valor `opaque` oculta todo lo que hay detrás del vídeo en la Página Web. El valor `transparent` permite que el fondo de la página web se muestre a través del vídeo, suponiendo que cualquier parte del vídeo es transparente.

<a id="Playing_MediaPlayer"></a>
## <a name="playing-mediaplayer-wmv-videos"></a>Reproducir vídeos de MediaPlayer ( *. wmv*)

En el procedimiento siguiente se muestra cómo reproducir un vídeo de Windows Media denominado *sample. wmv* que se encuentra en la carpeta *multimedia* .

1. Agregue la biblioteca de aplicaciones auxiliares Web de ASP.NET a su sitio web, tal como se describe en [instalación de aplicaciones auxiliares en un sitio de ASP.NET Web pages](https://go.microsoft.com/fwlink/?LinkId=252372), si no lo ha hecho ya.
2. Cree una nueva página denominada *MediaPlayerVideo. cshtml*.
3. Agregue el siguiente marcado a la página: 

    [!code-cshtml[Main](10-working-with-video/samples/sample5.cshtml)]
4. Ejecute la página en un explorador. El vídeo se carga y se reproduce automáticamente. 

    ![impresión](10-working-with-video/_static/image2.jpg "ch08_video -2. jpg")

Puede establecer `playCount` en un entero que indique el número de veces que se reproducirá el vídeo automáticamente:

[!code-cshtml[Main](10-working-with-video/samples/sample6.cshtml)]

El parámetro `uiMode` permite especificar qué controles se muestran en la interfaz de usuario. Puede establecer `uiMode` en `invisible`, `none`, `mini`o `full`. Si no especifica un parámetro de `uiMode`, el vídeo se mostrará con la ventana de estado, la barra de búsqueda, los botones de control y los controles de volumen, además de la ventana de vídeo. Estos controles también se mostrarán si usa el reproductor para reproducir un archivo de audio. A continuación se muestra un ejemplo de cómo usar el parámetro `uiMode`:

[!code-cshtml[Main](10-working-with-video/samples/sample7.cshtml)]

De forma predeterminada, el audio está encendido cuando el vídeo se reproduce. Puede silenciar el audio estableciendo el parámetro `mute` en true:

[!code-cshtml[Main](10-working-with-video/samples/sample8.cshtml)]

Puede controlar el nivel de audio del vídeo de MediaPlayer si establece el parámetro `volume` en un valor entre 0 y 100. El valor predeterminado es 50. Por ejemplo:

[!code-cshtml[Main](10-working-with-video/samples/sample9.cshtml)]

<a id="Playing_Silverlight"></a>
## <a name="playing-silverlight-videos"></a>Reproducir vídeos de Silverlight

Este procedimiento muestra cómo reproducir el vídeo contenido en una página de Silverlight *. xap* que se encuentra en una carpeta denominada *media*.

1. Agregue la biblioteca de aplicaciones auxiliares Web de ASP.NET a su sitio web, tal como se describe en [instalación de aplicaciones auxiliares en un sitio de ASP.NET Web pages](https://go.microsoft.com/fwlink/?LinkId=252372), si no lo ha hecho ya.
2. Cree una nueva página denominada *SilverlightVideo. cshtml*.
3. Agregue el siguiente marcado a la página: 

    [!code-cshtml[Main](10-working-with-video/samples/sample10.cshtml)]
4. Ejecute la página en un explorador. 

    ![impresión](10-working-with-video/_static/image3.jpg "ch08_video -3. jpg")

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Recursos adicionales

[Información general de Silverlight](https://msdn.microsoft.com/library/bb404700(VS.95).aspx)

[Atributos de etiqueta de INCRUSTAción y objeto Flash](http://kb2.adobe.com/cps/127/tn_12701.html)

[Etiquetas de parámetros del SDK de Windows Media Player 11](https://msdn.microsoft.com/library/aa392321(VS.85).aspx)
