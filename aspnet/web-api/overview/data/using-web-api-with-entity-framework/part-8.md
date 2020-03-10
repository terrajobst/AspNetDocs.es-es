---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-8
title: Mostrar detalles del elemento | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 06/16/2014
ms.assetid: 75ef94b1-bbec-4681-9210-452dba816144
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-8
msc.type: authoredcontent
ms.openlocfilehash: 3f48c5ad73ceb9a4873fbbb621b518553a498966
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78449005"
---
# <a name="display-item-details"></a>Mostrar detalles del elemento

por [Mike Wasson](https://github.com/MikeWasson)

[Descargar proyecto completado](https://github.com/MikeWasson/BookService)

En esta sección, agregará la capacidad de ver los detalles de cada libro. En App. js, agregue al siguiente código al modelo de vista:

[!code-javascript[Main](part-8/samples/sample1.js)]

En views/home/index. cshtml, agregue un elemento enlazado a datos al vínculo Details:

[!code-html[Main](part-8/samples/sample2.html?highlight=5)]

Esto enlaza el controlador de clic para el &lt;un elemento de&gt; a la función `getBookDetail` en el modelo de vista.

En el mismo archivo, reemplace la siguiente marca:

[!code-html[Main](part-8/samples/sample3.html)]

por este:

[!code-html[Main](part-8/samples/sample4.html)]

Este marcado crea una tabla que está enlazada a datos con las propiedades del `detail` observable en el modelo de vista.

La sintaxis "&lt;!--Ko--&gt;&quot; permite incluir un enlace de cobertura fuera de un elemento DOM. En este caso, el enlace de `if` hace que esta sección de marcado solo se muestre cuando `details` no es NULL.

[!code-html[Main](part-8/samples/sample5.html)]

Ahora, si ejecuta la aplicación y hace clic en uno de los vínculos de&quot; de detalles de &quot;, la aplicación mostrará los detalles del libro.

[![](part-8/_static/image2.png)](part-8/_static/image1.png)

> [!div class="step-by-step"]
> [Anterior](part-7.md)
> [Siguiente](part-9.md)
