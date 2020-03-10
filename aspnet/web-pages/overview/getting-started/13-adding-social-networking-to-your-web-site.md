---
uid: web-pages/overview/getting-started/13-adding-social-networking-to-your-web-site
title: Incorporación de redes sociales a sitios de ASP.NET Web Pages (Razor) | Microsoft Docs
author: Rick-Anderson
description: En este capítulo se explica cómo integrar su sitio con los servicios de redes sociales. En este capítulo, aprenderá a permitir que los usuarios marquen o vinculen su sitio Web...
ms.author: riande
ms.date: 02/21/2014
ms.assetid: 03c342f9-b35c-4d7c-b9ed-cd9aaaffedb6
msc.legacyurl: /web-pages/overview/getting-started/13-adding-social-networking-to-your-web-site
msc.type: authoredcontent
ms.openlocfilehash: 1637464b0473bba8133acbbf8918d92b4f552701
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78422959"
---
# <a name="adding-social-networking-to-aspnet-web-pages-razor-sites"></a>Incorporación de redes sociales a sitios de ASP.NET Web Pages (Razor)

por [Tom FitzMacken](https://github.com/tfitzmac)

> En este artículo se explica cómo agregar vínculos de redes sociales para Facebook, Twitter, Reddit y a las páginas de un sitio web de ASP.NET Web Pages (Razor) y cómo incluir fuentes de Twitter, tarjetas para jugadores de Xbox e imágenes de Gravatar.
> 
> Temas que se abordarán:
> 
> - Cómo permitir que los usuarios marquen o vinculen su sitio.
> - Cómo agregar una fuente de Twitter.
> - Cómo agregar un botón de Facebook **como** a las páginas.
> - Cómo representar imágenes de Gravatar.com.
> - Cómo mostrar una tarjeta de jugador de Xbox en el sitio.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versiones de software usadas en el tutorial
> 
> 
> - ASP.NET Web Pages (Razor) 2
> - Biblioteca auxiliar Web de ASP.NET (paquete NuGet)
>   
> 
> Este tutorial también funciona con ASP.NET Web Pages 3, excepto para las partes que usan la biblioteca auxiliar Web ASP.NET.

<a id="Linking_Your_Website"></a>
## <a name="linking-your-website-on-social-networking-sites"></a>Vincular el sitio web en sitios de redes sociales

Si hay personas como algo en el sitio, suelen querer compartirlo con amigos. Para facilitar esta tarea, puede mostrar glifos (iconos) en los que los usuarios pueden hacer clic para compartir una página en el sitio de Reddit, Facebook, Twitter o sitios similares.

Para mostrar estos glifos, agregue el ayudante de `LinkSharecode` a una página. Las personas que visitan su página pueden hacer clic en un glifo individual. Si tienen una cuenta con ese sitio de redes sociales, pueden publicar un vínculo a la página en ese sitio.

![Imagen 1](13-adding-social-networking-to-your-web-site/_static/image1.jpg)

1. Agregue la biblioteca de aplicaciones auxiliares Web de ASP.NET a su sitio web, tal como se describe en [instalación de aplicaciones auxiliares en un sitio de ASP.NET Web pages](https://go.microsoft.com/fwlink/?LinkId=252372), si aún no la ha agregado.-cree una página denominada *ListLinkShare. cshtml* y agregue el marcado siguiente:

    [!code-cshtml[Main](13-adding-social-networking-to-your-web-site/samples/sample1.cshtml)]

    En este ejemplo, cuando se ejecuta la aplicación auxiliar de `LinkShare`, el título de la página se pasa como un parámetro, que a su vez pasa el título de la página al sitio de redes sociales. Sin embargo, podría pasar cualquier cadena que desee. En este ejemplo también se especifica qué sitios de redes sociales se van a incluir en la lista. Puede especificar los sitios de redes sociales que son relevantes para su sitio.
2. Ejecute la página *ListLinkShare. cshtml* en un explorador. (Asegúrese de que la página esté seleccionada en el área de trabajo **archivos** antes de ejecutarla).
3. Haga clic en un glifo para uno de los sitios para los que se ha suscrito. El vínculo le lleva a la página del sitio de red social seleccionado en el que puede compartir un vínculo. Por ejemplo, si hace clic en el vínculo Reddit, se le dirigirá a la página `submit to reddit` del sitio web de Reddit.

     ![Imagen 2](13-adding-social-networking-to-your-web-site/_static/image2.jpg)

<a id="Adding_a_Twitter_Feed"></a>
## <a name="adding-a-twitter-feed"></a>Agregar una fuente de Twitter

Para obtener información sobre el uso de una aplicación auxiliar de Twitter compatible con la versión actual de la API de Twitter, consulte [aplicación auxiliar](../ui-layouts-and-themes/twitter-helper.md)de Twitter. En este ejemplo se muestra cómo escribir su propio ayudante para que pueda volver a usar fácilmente el código desde muchas páginas.

<a id="Displaying_a_Facebook_Button"></a>
## <a name="displaying-a-facebook-quotlikequot-button"></a>Mostrar un &quot;de Facebook como&quot; botón

En algunos casos, la mejor opción es obtener el código directamente del proveedor de redes sociales en lugar de depender de una aplicación auxiliar. Esto es especialmente cierto si el proveedor de red social actualiza sus opciones más rápidamente de lo que se actualiza el ayudante.

Para agregar características de Facebook (como el botón like) al sitio, puede recuperar fragmentos de código del sitio de [developers.Facebook.com](https://developers.facebook.com/) . En el sitio de Facebook, use sus herramientas para generar un fragmento de código relevante para su sitio.

El código resaltado siguiente es el código que se recuperó de la herramienta de botón like en el sitio developers.facebook.com. Debe proporcionar su propio identificador de aplicación.

[!code-html[Main](13-adding-social-networking-to-your-web-site/samples/sample2.html?highlight=7-14,16-17)]

<a id="Rendering_a_Gravatar_Image"></a>
## <a name="rendering-a-gravatar-image"></a>Representación de una imagen de Gravatar

Un *Gravatar* (un avatar &quot;globalmente reconocido&quot;) es una imagen que se puede usar en varios sitios web como el &#8212; Avatar, es decir, una imagen que le representa. Por ejemplo, un gravatar puede identificar a una persona en una publicación de foro, en un Comentario de blog, etc. (Puede registrar su propia Gravatar en el sitio web de Gravatar en [http://www.gravatar.com/](http://www.gravatar.com/)). Si desea mostrar imágenes junto a los nombres de las personas o las direcciones de correo electrónico de su sitio web, puede usar la aplicación auxiliar Gravatar.

En este ejemplo, se usa un único Gravatar que se representa a sí mismo. Otra forma de usar una gravatar es permitir que los usuarios especifiquen su dirección de Gravatar cuando se registren en su sitio. (Puede obtener información sobre cómo permitir que los usuarios se registren en [Agregar seguridad y pertenencia a un sitio ASP.NET Web pages](https://go.microsoft.com/fwlink/?LinkId=202904)). Después, cada vez que se muestra información para ese usuario, basta con agregar el Gravatar a la ubicación en la que se muestra el nombre del usuario.

1. Agregue la biblioteca de aplicaciones auxiliares Web de ASP.NET a su sitio web, tal como se describe en [instalación de aplicaciones auxiliares en un sitio de ASP.NET Web pages](https://go.microsoft.com/fwlink/?LinkId=252372), si no lo ha hecho ya.
2. Cree una nueva página web denominada *Gravatar. cshtml*.
3. Agregue el marcado siguiente al archivo: 

    [!code-cshtml[Main](13-adding-social-networking-to-your-web-site/samples/sample3.cshtml)]

    El método `Gravatar.GetHtml` muestra la imagen Gravatar en la página. Para cambiar el tamaño de la imagen, puede incluir un número como segundo parámetro. El tamaño predeterminado es 80. Los números menores que 80 hacen que la imagen sea más pequeña. Los números mayores que 80 hacen que la imagen sea más grande.
4. En los métodos de `Gravatar.GetHtml`, reemplace `<Your Gravatar account here>` por la dirección de correo electrónico que usa para la cuenta de Gravatar. (Si no tiene una cuenta de Gravatar, puede usar la dirección de correo electrónico de alguien que lo haga).
5. Ejecute la página en el explorador. En la página se muestran dos imágenes de Gravatar para la dirección de correo electrónico especificada. La segunda imagen es menor que la primera. 

    ![Imagen 4](13-adding-social-networking-to-your-web-site/_static/image3.jpg)

<a id="Displaying_an_Xbox_Gamer_Card"></a>
## <a name="displaying-an-xbox-gamer-card"></a>Mostrar una tarjeta de jugador de Xbox

Cuando las personas juegan juegos de Microsoft Xbox en línea, cada usuario tiene un identificador único. Las estadísticas se mantienen para cada jugador en forma de tarjeta de jugador, que muestra su reputación, puntuación de jugador y juegos reproducidos recientemente. Si es un jugador de Xbox, puede mostrar la tarjeta del jugador en las páginas de su sitio mediante la aplicación auxiliar de `GamerCard`.

1. Agregue la biblioteca de aplicaciones auxiliares Web de ASP.NET a su sitio web, tal como se describe en [instalación de aplicaciones auxiliares en un sitio de ASP.NET Web pages](https://go.microsoft.com/fwlink/?LinkId=252372), si no lo ha hecho ya.
2. Cree una nueva página denominada *XboxGamer. cshtml* y agregue el marcado siguiente.

    [!code-cshtml[Main](13-adding-social-networking-to-your-web-site/samples/sample4.cshtml)]

    La propiedad `GamerCard.GetHtml` se usa para especificar el alias de la tarjeta jugador que se va a mostrar.
3. Ejecute la página en el explorador. En la página se muestra la tarjeta Xbox jugador que especificó.

    ![Imagen 5](13-adding-social-networking-to-your-web-site/_static/image4.jpg)
