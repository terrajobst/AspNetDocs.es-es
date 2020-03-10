---
uid: web-pages/overview/routing/creating-readable-urls-in-aspnet-web-pages-sites
title: Creación de direcciones URL legibles en sitios de ASP.NET Web Pages (Razor) | Microsoft Docs
author: Rick-Anderson
description: En este artículo se describe el enrutamiento en un sitio web de ASP.NET Web Pages (Razor) y cómo esto le permite usar direcciones URL más legibles y mejores para SEO. Qué desea...
ms.author: riande
ms.date: 02/17/2014
ms.assetid: a8aac1ac-89de-4415-afe0-97a41c6423d2
msc.legacyurl: /web-pages/overview/routing/creating-readable-urls-in-aspnet-web-pages-sites
msc.type: authoredcontent
ms.openlocfilehash: 832db8e144cab730f16c78f67c12feb9b7c92c7c
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78509911"
---
# <a name="creating-readable-urls-in-aspnet-web-pages-razor-sites"></a>Creación de direcciones URL legibles en sitios ASP.NET Web Pages (Razor)

por [Tom FitzMacken](https://github.com/tfitzmac)

> En este artículo se describe el enrutamiento en un sitio web de ASP.NET Web Pages (Razor) y cómo esto le permite usar direcciones URL más legibles y mejores para SEO.
> 
> Temas que se abordarán:
> 
> - Cómo usa ASP.NET el enrutamiento para permitir el uso de direcciones URL más legibles y de búsqueda.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versiones de software usadas en el tutorial
> 
> 
> - ASP.NET Web Pages (Razor) 3
>   
> 
> Este tutorial también funciona con ASP.NET Web Pages 2.

## <a name="about-routing"></a>Acerca del enrutamiento

Las direcciones URL de las páginas de su sitio pueden afectar al grado de funcionamiento del sitio. Una dirección URL &quot;&quot; descriptivos puede facilitar a los usuarios el uso del sitio. También puede ayudar con la optimización del motor de búsqueda (SEO) para el sitio. Los sitios web de ASP.NET incluyen la capacidad de usar direcciones URL descriptivas automáticamente.

ASP.NET permite crear direcciones URL significativas que describen las acciones del usuario en lugar de apuntar simplemente a un archivo en el servidor. Tenga en cuenta estas direcciones URL para un blog ficticio:

- `http://www.contoso.com/Blog/blog.cshtml?categories=hardware`
- `http://www.contoso.com//Blog/blog.cshtml?startdate=2009-11-01&enddate=2009-11-30`

Compare esas direcciones URL con las siguientes:

- `http://www.contoso.com/Blog/categories/hardware/`
- `http://www.contoso.com/Blog/2009/November`

En el primer par, un usuario tendría que saber que el blog se muestra mediante la página *blog. cshtml* y, a continuación, tendría que crear una cadena de consulta que obtenga la categoría o el intervalo de fechas correctos. El segundo conjunto de ejemplos es mucho más fácil de entender y crear.

Las direcciones URL del primer ejemplo también señalan directamente a un archivo específico (*blog. cshtml*). Si por alguna razón el blog se moviera a otra carpeta del servidor, o si el blog se reescribió para usar una página diferente, los vínculos no funcionarán correctamente. El segundo conjunto de direcciones URL no apunta a una página específica, por lo que, aunque la implementación o la ubicación del blog cambien, las direcciones URL seguirán siendo válidas.

En ASP.NET Web Pages, puede crear direcciones URL más descriptivas, como las de los ejemplos anteriores, porque ASP.NET usa el *enrutamiento*. El enrutamiento crea una asignación lógica desde una dirección URL a una página (o páginas) que puede satisfacer la solicitud. Dado que la asignación es lógica (no física, a un archivo específico), el enrutamiento proporciona una gran flexibilidad en la forma de definir las direcciones URL para el sitio.

## <a name="how-routing-works"></a>Cómo funciona el enrutamiento

Cuando ASP.NET procesa una solicitud, lee la dirección URL para determinar cómo enrutarla. ASP.NET intenta hacer coincidir los segmentos individuales de la dirección URL con los archivos del disco, de izquierda a derecha. Si hay una coincidencia, todo lo restante en la dirección URL se pasa a la página como información de la *ruta de acceso*.

Imagine que alguien realiza una solicitud con esta dirección URL:

`http://www.contoso.com/a/b/c`

La búsqueda tiene el siguiente aspecto:

1. ¿Hay un archivo con la ruta de acceso y el nombre de */a/b/c.cshtml*? Si es así, ejecute esa página y no le pase ninguna información. De lo contrario...
2. ¿Hay un archivo con la ruta de acceso y el nombre de */a/b.cshtml*? Si es así, ejecute esa página y pase el valor `c` a ella. En caso contrario,...
3. ¿Hay un archivo con la ruta de acceso y el nombre de */a.cshtml*? Si es así, ejecute esa página y pase el valor `b/c` a ella.

Si la búsqueda no encuentra coincidencias exactas para los archivos *. cshtml* en sus carpetas especificadas, ASP.net continúa buscando estos archivos a su vez:

1. */a/b/c/default.cshtml* (sin información de ruta de acceso).
2. */a/b/c/index.cshtml* (sin información de ruta de acceso).

> [!NOTE]
> Para ser claros, las solicitudes de páginas específicas (es decir, las solicitudes que incluyen la extensión de nombre de archivo *. cshtml* ) funcionan del mismo modo que cabría esperar. Una solicitud como `http://www.contoso.com/a/b.cshtml` ejecutará la página *b. cshtml* .

Dentro de una página, puede obtener la información de la ruta de acceso a través de la propiedad `UrlData` de la página, que es un diccionario. Imagine que tiene un archivo llamado *ViewCustomers. cshtml* y que el sitio obtiene esta solicitud:

`http://mysite.com/myWebSite/ViewCustomers/1000`

Tal y como se describe en las reglas anteriores, la solicitud irá a la página. Dentro de la página, puede usar código similar al siguiente para obtener y mostrar la información de la ruta de acceso (en este caso, el valor &quot;1000&quot;):

[!code-html[Main](creating-readable-urls-in-aspnet-web-pages-sites/samples/sample1.html)]

> [!NOTE]
> Dado que el enrutamiento no implica nombres de archivo completos, puede haber ambigüedad si tiene páginas con el mismo nombre pero con extensiones de nombre de archivo diferentes (por ejemplo, la *página. cshtml* y la *página. html*). Con el fin de evitar problemas con el enrutamiento, es mejor asegurarse de que no tiene páginas en el sitio cuyos nombres solo difieren en su extensión.

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Recursos adicionales

[WebMatrix: direcciones URL, UrlData y enrutamiento para SEO](http://www.mikesdotnetting.com/Article/165/WebMatrix-URLs-UrlData-and-Routing-for-SEO). En esta entrada de blog, Mike enlazód, se proporcionan algunos detalles adicionales sobre cómo funciona el enrutamiento en ASP.NET Web Pages.
