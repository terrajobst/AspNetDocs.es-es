---
uid: web-pages/overview/performance-and-traffic/14-analyzing-traffic
title: Seguimiento de la información de los visitantes (análisis) de un sitio de ASP.NET Web Pages (Razor) | Microsoft Docs
author: Rick-Anderson
description: Una vez que haya llegado el sitio web, puede que desee analizar el tráfico del sitio Web.
ms.author: riande
ms.date: 02/17/2014
ms.assetid: 360bc6e1-84c5-4b8e-a84c-ea48ab807aa4
msc.legacyurl: /web-pages/overview/performance-and-traffic/14-analyzing-traffic
msc.type: authoredcontent
ms.openlocfilehash: 095a5572c755446e0661c052ca9de82d636429fd
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78421459"
---
# <a name="tracking-visitor-information-analytics-for-an-aspnet-web-pages-razor-site"></a>Seguimiento de la información de los visitantes (análisis) de un sitio de ASP.NET Web Pages (Razor)

por [Tom FitzMacken](https://github.com/tfitzmac)

> En este artículo se describe cómo usar una aplicación auxiliar para agregar análisis de sitios web a páginas en un sitio web de ASP.NET Web Pages (Razor).
> 
> Temas que se abordarán:
> 
> - Cómo enviar información sobre el tráfico de su sitio web a un proveedor de análisis.
> 
> Estas son las características de programación de ASP.NET que se introdujeron en el artículo:
> 
> - Aplicación auxiliar de `Analytics`.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versiones de software usadas en el tutorial
> 
> 
> - ASP.NET Web Pages (Razor) 2
> - Biblioteca de aplicaciones auxiliares Web de ASP.NET (paquete NuGet)

Analytics es un término general para la tecnología que mide el tráfico en su sitio web para que pueda entender cómo usan el sitio los usuarios. Muchos servicios de análisis están disponibles, incluidos los servicios de Google, Yahoo, StatCounter y otros.

La forma en que el análisis funciona es que se suscribe a una cuenta con el proveedor de análisis, donde se registra el sitio del que desea realizar un seguimiento. El proveedor le envía un fragmento de código JavaScript que incluye un identificador o un código de seguimiento para su cuenta. Agregue el fragmento de código de JavaScript a las páginas web del sitio del que desea realizar un seguimiento. (Normalmente, el fragmento de código de Analytics se agrega a un pie de página o a una página de diseño u otro marcado HTML que aparece en todas las páginas del sitio). Cuando los usuarios solicitan una página que contiene uno de estos fragmentos de código de JavaScript, el fragmento de código envía información sobre la página actual al proveedor de análisis, que registra varios detalles sobre la página.

Si desea ver las estadísticas del sitio, inicie sesión en el sitio web del proveedor de análisis. A continuación, puede ver todo tipo de informes sobre el sitio, como:

- El número de vistas de página para páginas individuales. Esto le indica aproximadamente el número de personas que visitan el sitio y qué páginas del sitio son las más populares.
- Cuánto tiempo gastan las personas en páginas específicas. Esto puede indicar si la Página principal mantiene el interés de los usuarios.
- En qué sitios se encontraban los usuarios antes de visitar el sitio. Esto le ayudará a comprender si el tráfico procede de vínculos, de búsquedas, etc.
- Cuando los usuarios visitan su sitio y cuánto tiempo permanecen.
- En qué países se encuentran los visitantes.
- Qué exploradores y sistemas operativos están usando los visitantes.

    ![Ch14traffic-1](14-analyzing-traffic/_static/image1.jpg)

## <a name="using-a-helper-to-add-analytics-to-a-page"></a>Usar una aplicación auxiliar para agregar análisis a una página

ASP.NET Web Pages incluye varias aplicaciones auxiliares de análisis (`Analytics.GetGoogleHtml`, `Analytics.GetYahooHtml`y `Analytics.GetStatCounterHtml`) que facilitan la administración de los fragmentos de código de JavaScript que se usan para el análisis. En lugar de averiguar cómo y dónde colocar el código de JavaScript, lo único que tiene que hacer es agregar el ayudante a una página. La única información que debe proporcionar es el nombre de la cuenta, el identificador o el código de seguimiento. (Para StatCounter, también tiene que proporcionar algunos valores adicionales).

En este procedimiento, creará una página de diseño que utiliza la aplicación auxiliar `GetGoogleHtml`. Si ya tiene una cuenta con uno de los otros proveedores de análisis, puede usar esa cuenta en su lugar y realizar pequeños ajustes según sea necesario.

> [!NOTE]
> Al crear una cuenta de análisis, debe registrar la dirección URL del sitio del que desea realizar el seguimiento. Si está probando todo en el equipo local, no realizará el seguimiento del tráfico real (el único tráfico es), por lo que no podrá registrar y ver las estadísticas del sitio. Pero este procedimiento muestra cómo agregar una aplicación auxiliar de análisis a una página. Al publicar el sitio, el sitio activo enviará información al proveedor de análisis.

1. Agregue la biblioteca de aplicaciones auxiliares Web de ASP.NET a su sitio web, tal como se describe en [instalación de aplicaciones auxiliares en un sitio de ASP.NET Web pages](https://go.microsoft.com/fwlink/?LinkId=252372), si aún no lo ha hecho.
2. Cree una cuenta con Google Analytics y registre el nombre de la cuenta.
3. Cree una página de diseño denominada *Analytics. cshtml* y agregue el marcado siguiente:

    [!code-cshtml[Main](14-analyzing-traffic/samples/sample1.cshtml)]

    > [!NOTE]
    > Debe realizar la llamada a la aplicación auxiliar de `Analytics` en el cuerpo de la página web (antes de la etiqueta de `</body>`). De lo contrario, el explorador no ejecutará el script.

    Si usa un proveedor de análisis diferente, utilice en su lugar una de las aplicaciones auxiliares siguientes:

    - (Yahoo) `@Analytics.GetYahooHtml("myaccount")`
    - (StatCounter) `@Analytics.GetStatCounterHtml("project", "security")`
4. Reemplace `myaccount` por el nombre de la cuenta, el identificador o el código de seguimiento que creó en el paso 1.
5. Ejecute la página en el explorador. (Asegúrese de que la página esté seleccionada en el área de trabajo **archivos** antes de ejecutarla).
6. En el explorador, vea el origen de la página. Podrá ver el código de análisis representado:

    [!code-html[Main](14-analyzing-traffic/samples/sample2.html)]
7. Inicie sesión en el sitio de Google Analytics y examine las estadísticas de su sitio. Si está ejecutando la página en un sitio activo, verá una entrada que registra la visita a la página.

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Recursos adicionales

- [Sitio de Google Analytics](https://www.google.com/analytics/)
- [Sitio de Yahoo! Web Analytics](http://help.yahoo.com/l/us/yahoo/ywa/)
- [Sitio de StatCounter](http://statcounter.com/)
