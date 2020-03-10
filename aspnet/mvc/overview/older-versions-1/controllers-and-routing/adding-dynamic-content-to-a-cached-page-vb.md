---
uid: mvc/overview/older-versions-1/controllers-and-routing/adding-dynamic-content-to-a-cached-page-vb
title: Agregar contenido dinámico a una página almacenada en caché (VB) | Microsoft Docs
author: microsoft
description: Obtenga información sobre cómo mezclar contenido dinámico y en caché en la misma página. La sustitución posterior a la caché permite mostrar contenido dinámico, como anuncios de banner o...
ms.author: riande
ms.date: 01/27/2009
ms.assetid: 68acd884-fb57-4486-a1be-aaa93e380780
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/adding-dynamic-content-to-a-cached-page-vb
msc.type: authoredcontent
ms.openlocfilehash: f2f4372498e5a38bbfcb96d6e9f6338b0ef4df1f
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78486967"
---
# <a name="adding-dynamic-content-to-a-cached-page-vb"></a>Agregar contenido dinámico a una página almacenada en caché (VB)

por [Microsoft](https://github.com/microsoft)

> Obtenga información sobre cómo mezclar contenido dinámico y en caché en la misma página. La sustitución posterior a la caché permite mostrar contenido dinámico, como anuncios de banner o elementos de noticias, dentro de una página que se ha almacenado en memoria caché.

Al aprovechar el almacenamiento en caché de resultados, puede mejorar drásticamente el rendimiento de una aplicación ASP.NET MVC. En lugar de volver a generar una página cada vez que se solicita la página, la página puede generarse una vez y almacenarse en la memoria caché para varios usuarios.

Pero hay un problema. ¿Qué ocurre si necesita mostrar contenido dinámico en la página? Por ejemplo, Imagine que desea mostrar un anuncio de banner en la página. No desea almacenar en caché el anuncio del banner para que todos los usuarios vean el mismo anuncio. No podría ganar ningún dinero.

Afortunadamente, hay una solución sencilla. Puede sacar partido de una característica del marco de trabajo de ASP.NET denominada *sustitución posterior a la caché*. La sustitución posterior a la caché permite sustituir el contenido dinámico de una página almacenada en la memoria caché.

Normalmente, cuando la salida almacena en caché una página mediante el atributo &lt;OutputCache&gt;, la página se almacena en caché tanto en el servidor como en el cliente (el explorador Web). Cuando se usa la sustitución posterior a la caché, una página se almacena en caché solo en el servidor.

#### <a name="using-post-cache-substitution"></a>Usar la sustitución posterior a la caché

El uso de la sustitución posterior a la caché requiere dos pasos. En primer lugar, debe definir un método que devuelva una cadena que represente el contenido dinámico que desea mostrar en la página almacenada en caché. A continuación, llame al método HttpResponse. WriteSubstitution () para insertar el contenido dinámico en la página.

Imagine, por ejemplo, que desea mostrar aleatoriamente diferentes elementos de noticias en una página almacenada en caché. La clase de la lista 1 expone un método único, denominado RenderNews (), que devuelve aleatoriamente un elemento de noticias de una lista de tres elementos de noticias.

**Lista 1: Models\News.vb**

[!code-vb[Main](adding-dynamic-content-to-a-cached-page-vb/samples/sample1.vb)]

Para aprovechar las ventajas de la sustitución posterior a la caché, se llama al método HttpResponse. WriteSubstitution (). El método WriteSubstitution () configura el código para reemplazar una región de la página almacenada en caché por contenido dinámico. El método WriteSubstitution () se usa para mostrar el elemento de noticias aleatorio en la vista en la lista 2.

**Lista 2: Views\Home\Index.aspx**

[!code-aspx[Main](adding-dynamic-content-to-a-cached-page-vb/samples/sample2.aspx)]

El método RenderNews se pasa al método WriteSubstitution (). Observe que no se llama al método RenderNews. En su lugar, se pasa una referencia al método a WriteSubstitution () con la ayuda del operador AddressOf.

La vista de índice está almacenada en caché. El controlador devuelve la vista en la lista 3. Observe que la acción index () está decorada con un atributo &lt;OutputCache&gt; que hace que la vista de índice se almacene en caché durante 60 segundos.

**Lista 3: Controllers\HomeController.vb**

[!code-vb[Main](adding-dynamic-content-to-a-cached-page-vb/samples/sample3.vb)]

Aunque la vista de índice está almacenada en caché, se muestran diferentes elementos de noticias aleatorios al solicitar la página de índice. Cuando se solicita la página de índice, la hora que se muestra en la página no cambia durante 60 segundos (vea la figura 1). El hecho de que no cambie la hora demuestra que la página se almacena en caché. Sin embargo, el contenido insertado por el método WriteSubstitution (), el elemento de noticias aleatorio, cambia con cada solicitud.

**Figura 1: inserción de elementos de noticias dinámicos en una página almacenada en caché**

![clip_image002](adding-dynamic-content-to-a-cached-page-vb/_static/image1.jpg)

#### <a name="using-post-cache-substitution-in-helper-methods"></a>Usar la sustitución posterior a la caché en métodos auxiliares

Una manera más fácil de aprovechar la sustitución posterior a la caché es encapsular la llamada al método WriteSubstitution () dentro de un método auxiliar personalizado. Este enfoque se ilustra mediante el método auxiliar de la lista 4.

**Lista 4: Helpers\AdHelper.vb**

[!code-vb[Main](adding-dynamic-content-to-a-cached-page-vb/samples/sample4.vb)]

La lista 4 contiene un módulo de Visual Basic que expone dos métodos: RenderBanner () y RenderBannerInternal (). El método RenderBanner () representa el método auxiliar real. Este método amplía la clase estándar ASP.NET MVC HtmlHelper para que pueda llamar a HTML. RenderBanner () en una vista como cualquier otro método auxiliar.

El método RenderBanner () llama al método HttpResponse. WriteSubstitution () pasando el método RenderBannerInternal () al método WriteSubstitution ().

El método RenderBannerInternal () es un método privado. Este método no se expondrá como método auxiliar. El método RenderBannerInternal () devuelve aleatoriamente una imagen de anuncio de banner de una lista de tres imágenes de anuncio de banner.

La vista de índice modificada de la lista 5 muestra cómo se puede usar el método auxiliar RenderBanner (). Tenga en cuenta que se incluye una directiva &lt;% @ Import%&gt; adicional en la parte superior de la vista para importar el espacio de nombres MvcApplication1. helpers. Si no importa este espacio de nombres, el método RenderBanner () no aparecerá como un método en la propiedad HTML.

**Lista 5 – Views\Home\Index.aspx (con el método RenderBanner ())**

[!code-aspx[Main](adding-dynamic-content-to-a-cached-page-vb/samples/sample5.aspx)]

Cuando solicita la página representada por la vista de la lista 5, se muestra un anuncio de banner diferente con cada solicitud (consulte la figura 2). La página se almacena en la memoria caché, pero el método auxiliar RenderBanner () inserta dinámicamente el anuncio del banner.

**Figura 2: la vista de índice que muestra un anuncio de banner aleatorio**

![clip_image004](adding-dynamic-content-to-a-cached-page-vb/_static/image2.jpg)

#### <a name="summary"></a>Resumen

En este tutorial se explica cómo se puede actualizar dinámicamente el contenido de una página almacenada en caché. Aprendió a usar el método HttpResponse. WriteSubstitution () para permitir que el contenido dinámico se inserte en una página almacenada en caché. También aprendió a encapsular la llamada al método WriteSubstitution () dentro de un método auxiliar HTML.

Aproveche el almacenamiento en caché siempre que sea posible: puede tener un impacto drástico en el rendimiento de las aplicaciones Web. Como se explica en este tutorial, puede aprovechar las ventajas del almacenamiento en caché incluso cuando necesite Mostrar contenido dinámico en las páginas.

> [!div class="step-by-step"]
> [Anterior](improving-performance-with-output-caching-vb.md)
> [Siguiente](creating-a-controller-vb.md)
