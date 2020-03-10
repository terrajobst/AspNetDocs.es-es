---
uid: web-pages/overview/mobile/rendering-aspnet-web-pages-sites-for-mobile-devices
title: Sitios de ASP.NET Web Pages de representación (Razor) para dispositivos móviles | Microsoft Docs
author: Rick-Anderson
description: 'En este artículo se describe cómo crear páginas en un sitio de ASP.NET Web Pages (Razor) que se representará correctamente en dispositivos móviles. Qué aprenderá: Cómo...'
ms.author: riande
ms.date: 02/17/2014
ms.assetid: f15ab392-c05e-4269-83bf-7c6d2b8c8ec8
msc.legacyurl: /web-pages/overview/mobile/rendering-aspnet-web-pages-sites-for-mobile-devices
msc.type: authoredcontent
ms.openlocfilehash: c012348d65e48a275cb0e4808fef2a7f31e5fb33
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78454357"
---
# <a name="rendering-aspnet-web-pages-razor-sites-for-mobile-devices"></a>Sitios de ASP.NET Web Pages de representación (Razor) para dispositivos móviles

por [Tom FitzMacken](https://github.com/tfitzmac)

> En este artículo se describe cómo crear páginas en un sitio de ASP.NET Web Pages (Razor) que se representará correctamente en dispositivos móviles.
> 
> Temas que se abordarán:
> 
> - Cómo usar una Convención de nomenclatura para especificar que una página está diseñada específicamente para dispositivos móviles.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versiones de software usadas en el tutorial
> 
> 
> - ASP.NET Web Pages (Razor) 3
>   
> 
> Este tutorial también funciona con ASP.NET Web Pages 2.

ASP.NET Web Pages le permite crear pantallas personalizadas para representar el contenido en dispositivos móviles o en otros dispositivos.

La manera más sencilla de crear una página específica del dispositivo en un sitio ASP.NET Web Pages es usar un patrón de nomenclatura de archivos como este: *filename. Mobile. cshtml*. Puede crear dos versiones de una página (por ejemplo, una denominada *archivo. cshtml* y otra denominada *archivo. Mobile. cshtml*). En tiempo de ejecución, cuando un dispositivo móvil solicita *archivo. cshtml*, ASP.net representa el contenido de *archivo. Mobile. cshtml*. De lo contrario, se representa el *archivo. cshtml* .

En el ejemplo siguiente se muestra cómo habilitar la representación móvil mediante la adición de una página de contenido para dispositivos móviles. *Página1. cshtml* contiene contenido más una barra lateral de navegación. *Page1. Mobile. cshtml* contiene el mismo contenido, pero omite la barra lateral.

1. En un sitio de ASP.NET Web Pages, cree un archivo denominado *Page1. cshtml* y reemplace el contenido actual por el marcado siguiente.

    [!code-html[Main](rendering-aspnet-web-pages-sites-for-mobile-devices/samples/sample1.html)]
2. Cree un archivo denominado *Page1. Mobile. cshtml* y reemplace el contenido existente por el marcado siguiente. Tenga en cuenta que la versión móvil de la página omite la sección de navegación para una mejor representación en una pantalla más pequeña.

    [!code-html[Main](rendering-aspnet-web-pages-sites-for-mobile-devices/samples/sample2.html)]
3. Ejecute un explorador de escritorio y vaya a *página1. cshtml*. ![mobilesites-1](rendering-aspnet-web-pages-sites-for-mobile-devices/_static/image1.png)
4. Ejecute un explorador móvil (o un emulador de dispositivos móviles) y vaya a *página1. cshtml*. (Tenga en cuenta que no incluye *. Mobile.* como parte de la dirección URL). Aunque la solicitud es a *Page1. cshtml*, ASP.net representa *página1. Mobile. cshtml*.

    ![mobilesites-2](rendering-aspnet-web-pages-sites-for-mobile-devices/_static/image2.png)

> [!NOTE]
> Para probar páginas móviles, puede usar un simulador de dispositivos móviles que se ejecute en un equipo de escritorio. Esta herramienta le permite probar las páginas web tal y como se verían en los dispositivos móviles (es decir, normalmente con un área de visualización mucho más pequeña). Un ejemplo de un simulador es el [complemento de conmutador de agente de usuario](http://addons.mozilla.org/firefox/addon/user-agent-switcher/) para Mozilla Firefox, que permite emular varios exploradores móviles desde una versión de escritorio de Firefox.

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Recursos adicionales

[Emulador de Windows Phone](https://msdn.microsoft.com/library/ff402563(v=VS.92).aspx)
