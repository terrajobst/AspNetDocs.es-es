---
uid: web-pages/overview/performance-and-traffic/15-caching-to-improve-the-performance-of-your-website
title: Almacenamiento en caché de datos en un sitio ASP.NET Web Pages (Razor) para mejorar el rendimiento | Microsoft Docs
author: Rick-Anderson
description: Puede acelerar el sitio web si lo almacena, es decir, almacenar en caché los resultados de los datos que, normalmente, tardarán un tiempo considerable en recuperar o procesar...
ms.author: riande
ms.date: 02/14/2014
ms.assetid: 961e525b-7700-469e-8a68-d7010b6fb68c
msc.legacyurl: /web-pages/overview/performance-and-traffic/15-caching-to-improve-the-performance-of-your-website
msc.type: authoredcontent
ms.openlocfilehash: 01796d3ca699a6af5d9162b22a926551435c2040
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78521161"
---
# <a name="caching-data-in-an-aspnet-web-pages-razor-site-for-better-performance"></a>Almacenamiento en caché de datos en un sitio ASP.NET Web Pages (Razor) para mejorar el rendimiento

por [Tom FitzMacken](https://github.com/tfitzmac)

> En este artículo se explica cómo usar una aplicación auxiliar para almacenar en caché información para obtener un rendimiento más rápido en un sitio web de ASP.NET Web Pages (Razor). Puede acelerar el sitio web porque lo almacena &#8212; , almacenar en caché &#8212; los resultados de los datos que normalmente tardarían un tiempo considerable en recuperarse o procesarse y que no cambian a menudo.
> 
> **Lo que aprenderá:** 
> 
> - Cómo usar el almacenamiento en caché para mejorar la capacidad de respuesta de su sitio Web.
> 
> Estas son las características de ASP.NET presentadas en el artículo:
> 
> - Aplicación auxiliar de `WebCache`.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versiones de software usadas en el tutorial
> 
> 
> - ASP.NET Web Pages (Razor) 3
>   
> 
> Este tutorial también funciona con ASP.NET Web Pages 2.

Cada vez que alguien solicita una página de su sitio, el servidor Web tiene que hacer algún trabajo para completar la solicitud. En algunas de las páginas, es posible que el servidor tenga que realizar tareas que tarden un período de tiempo (comparativamente), como la recuperación de datos de una base de datos. Incluso si estas tareas no tardan mucho en términos absolutos, si el sitio experimenta una gran cantidad de tráfico, toda una serie de solicitudes individuales que hacen que el servidor Web realice la tarea complicada o lenta puede Agregar un gran número de trabajo. En última instancia, esto puede afectar al rendimiento del sitio.

Una manera de mejorar el rendimiento de su sitio web en circunstancias como esta es almacenar los datos en caché. Si el sitio obtiene solicitudes repetidas para la misma información y no es necesario modificar la información para cada persona, y no es sensible a la hora, en lugar de volver a obtenerla o recalcularla, puede capturar los datos una vez y, después, almacenar los resultados. La próxima vez que llegue una solicitud para esa información, solo tendrá que salir de la memoria caché.

En general, se almacena en caché información que no cambia con frecuencia. Cuando se coloca información en la memoria caché, se almacena en memoria en el servidor Web. Puede especificar cuánto tiempo se debe almacenar en caché, de segundos a días. Cuando expire el período de almacenamiento en caché, la información se quitará automáticamente de la memoria caché.

> [!NOTE]
> Es posible que se quiten las entradas de la memoria caché por motivos distintos de los que han expirado. Por ejemplo, el servidor Web podría quedarse sin memoria temporalmente y una forma de recuperar memoria es producir entradas fuera de la memoria caché. Como podrá ver, incluso si ha colocado información en la memoria caché, tendrá que comprobar que todavía está allí cuando la necesite.

Imagine que su sitio web tiene una página que muestra la temperatura actual y la previsión meteorológica. Para obtener este tipo de información, puede enviar una solicitud a un servicio externo. Puesto que esta información no cambia mucho (en un período de dos horas, por ejemplo) y dado que las llamadas externas requieren tiempo y ancho de banda, es un buen candidato para el almacenamiento en caché.

## <a name="adding-caching-to-a-page"></a>Agregar almacenamiento en caché a una página

ASP.NET incluye una aplicación auxiliar `WebCache` que facilita la tarea de agregar almacenamiento en caché al sitio y agregar datos a la memoria caché. En este procedimiento, creará una página que almacena en caché la hora actual. No se trata de un ejemplo real, ya que la hora actual es algo que cambia a menudo y que, además, no es complejo de calcular. Sin embargo, es una buena manera de ilustrar el almacenamiento en caché en acción.

1. Agregue una nueva página denominada *webcache. cshtml* al sitio Web.
2. Agregue el código y el marcado siguientes a la página:

    [!code-cshtml[Main](15-caching-to-improve-the-performance-of-your-website/samples/sample1.cshtml)]

    Al almacenar datos en caché, se coloca en la memoria caché con un nombre que es único en todo el sitio Web. En este caso, usará una entrada de caché denominada `CachedTime`. Este es el `cacheItemKey` que se muestra en el ejemplo de código.

    En primer lugar, el código lee la entrada de caché de `CachedTime`. Si se devuelve un valor (es decir, si la entrada de la caché no es null), el código simplemente establece el valor de la variable de tiempo en los datos de la memoria caché.

    Sin embargo, si la entrada de caché no existe (es decir, es null), el código establece el valor de tiempo, lo agrega a la memoria caché y establece un valor de expiración en un minuto. Después de un minuto, se descarta la entrada de caché. (El valor de expiración predeterminado para un elemento de la memoria caché es de 20 minutos). El comando `WebCache.Set(cacheItemKey, time, 1, false)` muestra cómo agregar el valor de hora actual a la memoria caché y establecer su expiración en 1 minuto. Establecer el parámetro *SlidingExpiration* en `false` significa que la hora de expiración no se renueva cada vez que se solicita. Expirará exactamente 1 minuto después de que se agregara originalmente a la memoria caché. Si establece este valor en `true` se restablecerá el tiempo de expiración de 1 minuto cada vez que se solicite el valor de la memoria caché.

    Este código muestra el patrón que se debe usar siempre al almacenar datos en caché. Antes de sacar algo de la memoria caché, compruebe siempre si el método `WebCache.Get` ha devuelto null. Recuerde que es posible que la entrada de caché haya expirado o que se haya quitado por algún otro motivo, por lo que nunca se garantiza que una entrada determinada esté en la memoria caché.
3. Ejecute *webcache. cshtml* en un explorador. (Asegúrese de que la página esté seleccionada en el área de trabajo **archivos** antes de ejecutarla). La primera vez que se solicita la página, los datos de hora no están en la memoria caché y el código tiene que agregar el valor de tiempo a la memoria caché.

    ![cache-1](15-caching-to-improve-the-performance-of-your-website/_static/image1.jpg)
4. Actualice *webcache. cshtml* en el explorador. Esta vez, los datos de hora están en la memoria caché. Tenga en cuenta que la hora no ha cambiado desde la última vez que vio la página.

    ![cache-2](15-caching-to-improve-the-performance-of-your-website/_static/image2.jpg)
5. Espere un minuto para que se vacíe la memoria caché y, a continuación, actualice la página. La página vuelve a indicar que los datos de hora no se encontraron en la memoria caché y la hora actualizada se agrega a la caché.

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Recursos adicionales

- [Mostrar datos en un gráfico](https://go.microsoft.com/fwlink/?LinkId=202895)
- [Referencia de API de webcache](https://msdn.microsoft.com/library/system.web.helpers.webcache(v=vs.99).aspx) (MSDN)
