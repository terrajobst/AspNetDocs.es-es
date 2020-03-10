---
uid: web-pages/overview/ui-layouts-and-themes/twitter-helper
title: Aplicación auxiliar de Twitter con ASP.NET Web Pages | Microsoft Docs
author: Rick-Anderson
description: En este tema y aplicación se muestra cómo agregar una aplicación auxiliar de Twitter a su proyecto WebMatrix 3. Contiene el código auxiliar de Twitter y muestra cómo llamar a la aplicación auxiliar...
ms.author: riande
ms.date: 11/26/2018
ms.assetid: c1a1244e-b9c8-42e6-a00b-8456a4ec027c
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/twitter-helper
msc.type: authoredcontent
ms.openlocfilehash: 76e32b7c808467a9a87c70017dac02bdb895e1df
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78518623"
---
# <a name="twitter-helper-with-aspnet-web-pages"></a>Asistente de Twitter con ASP.NET Web Pages

por [Tom FitzMacken](https://github.com/tfitzmac)

> [!IMPORTANT]
> Las aplicaciones auxiliares de Twitter están obsoletas. Para ver las últimas herramientas de Engagement de Twitter para sitios web, consulte [información general de Twitter para sitios web](https://developer.twitter.com/en/docs/twitter-for-websites/overview).

> En este tema y aplicación se muestra cómo agregar una aplicación auxiliar de Twitter a su proyecto WebMatrix 3. Contiene el código auxiliar de Twitter y muestra cómo llamar a los métodos auxiliares.
> 
> Este código para el archivo Twitter. cshtml fue desarrollado por **Tian pan** de Microsoft.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versiones de software usadas en el tutorial
> 
> 
> - ASP.NET Web Pages (Razor) 3
>   
> 
> Este tutorial también funciona con ASP.NET Web Pages 2.

## <a name="introduction"></a>Introducción

En este tema se muestra cómo agregar una aplicación auxiliar de Twitter a la aplicación y usar sintaxis Razor para llamar a los métodos auxiliares. El ayudante de Twitter facilita la incorporación de los botones y widgets de Twitter en la aplicación. Para usar un widget de Twitter, como la escala de tiempo de un usuario o los resultados de búsqueda de un hashtag, primero debe crear el [Widget en Twitter](https://twitter.com/settings/widgets). Después de crear el widget, recibirá un identificador de widget. Pasa este identificador de widget como parámetro al llamar a los métodos auxiliares que muestran widget.

Este tema se ha escrito para la versión 1,1 de la API de Twitter. Al agregar directamente el código auxiliar de Twitter al proyecto, puede actualizar el código auxiliar si cambia la API de Twitter.

Para obtener información sobre cómo instalar WebMatrix, consulte [Introducción a ASP.NET Web Pages 2-Introducción](../getting-started/introducing-aspnet-web-pages-2/getting-started.md).

## <a name="add-twitter-helper-to-your-project"></a>Incorporación de la aplicación auxiliar de Twitter al proyecto

Para agregar la aplicación auxiliar de Twitter, en primer lugar, agregue una carpeta denominada **App\_código** al proyecto. A continuación, cree un archivo denominado **Twitter. cshtml**.

![Carpeta App_Code](twitter-helper/_static/image1.png)

Reemplace el código predeterminado de Twitter. cshtml por el código siguiente.

[!code-cshtml[Main](twitter-helper/samples/sample1.cshtml)]

## <a name="call-twitter-methods-from-your-web-pages"></a>Llamar a métodos de Twitter desde las páginas web

En el ejemplo siguiente se muestra cómo usar los métodos auxiliares de Twitter desde una página del proyecto. En el proyecto, querrá reemplazar los valores de parámetro por valores que sean relevantes para sus necesidades. Puede usar los identificadores de widget proporcionados para explorar cómo funcionan los métodos, pero querrá generar sus propios widgets para el proyecto.

No todos los parámetros que se muestran a continuación son obligatorios. Los parámetros opcionales se usan para personalizar cómo se muestra el botón o el widget. Por ejemplo, el botón seguir solo requiere que el nombre de usuario siga, pero en el ejemplo se muestra cómo incluir el número de seguidores y cómo especificar el tamaño del botón y el idioma.

[!code-html[Main](twitter-helper/samples/sample2.html)]

## <a name="see-the-results"></a>Ver los resultados

El código anterior genera los siguientes botones y widgets. Estos botones y widgets son totalmente funcionales, no capturas de pantallas. El botón siguiente se muestra en Español porque el parámetro de idioma se estableció en **es**.

### <a name="follow-button"></a>Botón seguir

[Siga @aspnet)](https://twitter.com/aspnet)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + '://platform.twitter.com/widgets.js'; fjs.parentNode.insertBefore(js, fjs); } }(document, 'script', 'twitter-wjs');</script>`

### <a name="tweet-button"></a>Botón Tweet

[Tweet](https://twitter.com/share)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + '://platform.twitter.com/widgets.js'; fjs.parentNode.insertBefore(js, fjs); } }(document, 'script', 'twitter-wjs');</script>`

### <a name="user-timeline-profile"></a>Escala de tiempo de usuario (perfil)

[Tweets por @aspnet](https://twitter.com/aspnet)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>`

### <a name="favorites"></a>Favoritos

[Tweets favoritos por @Microsoft](https://twitter.com/Microsoft/favorites)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>`

### <a name="list"></a>Lista

[Tweets de @Microsoft/MS\_bandas\_consumidor](https://twitter.com/microsoft/ms-consumer-brands/)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>`

### <a name="search"></a>Buscar

[Tweets sobre &quot;#asp .net&quot;](https://twitter.com/search?q=%23asp.net)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>`
