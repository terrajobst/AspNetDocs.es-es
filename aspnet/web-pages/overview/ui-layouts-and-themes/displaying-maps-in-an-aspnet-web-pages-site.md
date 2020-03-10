---
uid: web-pages/overview/ui-layouts-and-themes/displaying-maps-in-an-aspnet-web-pages-site
title: Visualización de mapas en un sitio de ASP.NET Web Pages (Razor) | Microsoft Docs
author: Rick-Anderson
description: En este artículo se explica cómo mostrar mapas interactivos en páginas en un sitio web de ASP.NET Web Pages (Razor) en función de los servicios de asignación proporcionados por Bing, Google, MA...
ms.author: riande
ms.date: 02/20/2014
ms.assetid: b5c268dd-ca6a-4562-b94c-a220fcf01f58
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/displaying-maps-in-an-aspnet-web-pages-site
msc.type: authoredcontent
ms.openlocfilehash: 36f3b753cf312504892872ff54bef49854588990
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78518725"
---
# <a name="displaying-maps-in-an-aspnet-web-pages-razor-site"></a>Visualización de mapas en un sitio de ASP.NET Web Pages (Razor)

por [Tom FitzMacken](https://github.com/tfitzmac)

> En este artículo se explica cómo mostrar mapas interactivos en páginas en un sitio web de ASP.NET Web Pages (Razor) en función de los servicios de asignación proporcionados por Bing, Google, MapQuest y Yahoo.
> 
> Temas que se abordarán:
> 
> - Cómo generar una asignación basada en una dirección.
> - Cómo generar una asignación basada en las coordenadas de latitud y longitud.
> - Cómo registrar una cuenta de desarrollador de Bing Maps y obtener una clave para usarla con mapas de Bing.
> 
> Esta es la característica ASP.NET presentada en el artículo:
> 
> - Aplicación auxiliar de `Maps`.
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

En las páginas web, puede mostrar las asignaciones en una página mediante `Maps` aplicación auxiliar. Puede generar asignaciones basadas en una dirección o en un conjunto de coordenadas de longitud y latitud. La clase `Maps` le permite llamar a motores de mapas populares, como Bing, Google, MapQuest y Yahoo.

Los pasos para agregar la asignación a una página son los mismos independientemente de los motores de mapa a los que llame. Solo tiene que agregar una referencia de archivo JavaScript que haga que los métodos disponibles muestren el mapa y, a continuación, llamar a los métodos de la aplicación auxiliar `Maps`.

Puede elegir un servicio de asignación basado en el `Maps` método auxiliar que use. Puede usar cualquiera de estos:

- `Maps.GetBingHtml`
- `Maps.GetGoogleHtml`
- `Maps.GetYahooHtml`
- `Maps.GetMapQuestHtml`

## <a name="installing-the-pieces-you-need"></a>Instalación de las piezas necesarias

Para mostrar las asignaciones, necesita estos elementos:

- Aplicación auxiliar de `Maps`. Esta aplicación auxiliar está en la versión 2 de la biblioteca de aplicaciones auxiliares Web de ASP.NET. Si aún no ha agregado la biblioteca, puede instalarla en el sitio como un paquete NuGet. Para obtener más información, consulte [instalación de aplicaciones auxiliares en un sitio de ASP.NET Web pages](https://go.microsoft.com/fwlink/?LinkId=252372). (En la galería, busque el paquete de `microsoft-web-helpers`).
- Biblioteca jQuery. Algunas de las plantillas de sitio de WebMatrix ya incluyen bibliotecas de jQuery en sus carpetas de *scripts* . Si no tiene estas bibliotecas, puede descargar la biblioteca de jQuery más reciente directamente desde el sitio de [jQuery.org](http://jQuery.org) . También puede crear un sitio nuevo mediante una plantilla (por ejemplo, la plantilla del **sitio de inicio** ) y, a continuación, copiar los archivos de jQuery desde ese sitio al sitio actual.

Por último, si desea utilizar mapas de Bing, primero debe crear una cuenta (gratuita) y obtener una clave. Para obtener una clave, siga estos pasos:

1. Cree una cuenta en la [cuenta de desarrollador de Bing Maps](https://www.microsoft.com/maps/developers/web.aspx). También debe tener un cuenta de Microsoft (Windows Live ID).

    Puede especificar que desea utilizar la clave para **evaluar y probar**. Si va a probar la función de asignación en su propio equipo mediante WebMatrix y IIS Express, vaya al área de trabajo del **sitio** y anote la dirección URL del sitio (por ejemplo, `http://localhost:50408`, aunque es probable que el número de Puerto sea diferente). Puede usar esta dirección *localhost* como sitio al registrarse.
2. Una vez que se haya registrado para una cuenta, vaya al centro de cuentas de Bing Maps y haga clic en **crear o ver claves**:

    ![asignación-2](displaying-maps-in-an-aspnet-web-pages-site/_static/image1.png)
3. Grabe la clave que Bing crea.

## <a name="creating-a-map-based-on-an-address-using-google"></a>Crear una asignación basada en una dirección (mediante Google)

En el ejemplo siguiente se muestra cómo crear una página que representa una asignación basada en una dirección. En este ejemplo se muestra cómo usar Google Maps.

1. Cree un archivo denominado *MapAddress. cshtml* en la raíz del sitio. En esta página se generará una asignación basada en una dirección que se le pase.
2. Copie el siguiente código en el archivo, sobrescribiendo el contenido existente.

    [!code-cshtml[Main](displaying-maps-in-an-aspnet-web-pages-site/samples/sample1.cshtml)]

    Tenga en cuenta las siguientes características de la página:

    - El elemento `<script>` del elemento `<head>`. En el ejemplo, el elemento `<script>` hace referencia al archivo *jQuery-1.6.4. min. js* , que es una versión reducida (comprimida) de la biblioteca de jQuery, versión 1.6.4. Tenga en cuenta que la referencia supone que el archivo *. js* está en la carpeta *scripts* del sitio. 

        > [!NOTE]
        > Si usa una versión diferente de la biblioteca de jQuery, asegúrese de que está apuntando correctamente a esa versión.
    - La llamada a la `@Maps.GetGoogleHtml` en el cuerpo de la página. Para asignar una dirección, debe pasar una cadena de dirección. Los métodos para los demás motores de mapa funcionan de manera similar (`@Maps.GetYahooHtml`, `@Maps.GetMapQuestHtml`).
3. Ejecute la página y escriba una dirección. La página muestra un mapa, basado en Google Maps, que muestra la ubicación que especificó.

     ![asignación-1](displaying-maps-in-an-aspnet-web-pages-site/_static/image2.png)

## <a name="creating-a-map-based-on-latitude-and-longitude-coordinates-using-bing"></a>Crear una asignación basada en las coordenadas de latitud y longitud (mediante Bing)

En este ejemplo se muestra cómo crear una asignación basada en coordenadas. En este ejemplo se muestra cómo usar Bing Maps y cómo incluir la clave de Bing. (Puede crear una asignación basada en coordenadas utilizando también los otros motores de mapa, sin usar una clave de Bing).

1. Cree un archivo denominado *MapCoordinates. cshtml* en la raíz del sitio y reemplace el contenido existente por el siguiente código y marcado:

    [!code-cshtml[Main](displaying-maps-in-an-aspnet-web-pages-site/samples/sample2.cshtml)]
2. Reemplace `your-key-here` por la clave de Bing Maps que ha generado anteriormente.
3. Ejecute la página *MapCoordinates. cshtml* , escriba las coordenadas de latitud y longitud y, a continuación, haga clic en el **mapa** . . (Si no conoce ninguna de las coordenadas, intente lo siguiente. Se trata de una ubicación en el campus de Microsoft Redmond).

   - Latitud: 47.6781005859375
   - Longitud:-122.158317565918

     La página se muestra con las coordenadas especificadas.

     ![asignación-3](displaying-maps-in-an-aspnet-web-pages-site/_static/image3.png)

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Recursos adicionales

[Referencia de la API de Microsoft. Maps](https://msdn.microsoft.com/library/gg427611.aspx)
