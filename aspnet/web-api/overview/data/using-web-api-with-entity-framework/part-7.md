---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-7
title: Crear la vista (UI) | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 06/16/2014
ms.assetid: b2445062-a1fe-4133-8994-f510280f6d9a
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-7
msc.type: authoredcontent
ms.openlocfilehash: 62c4523c2c6fb399cfbc3716309a1379996d601c
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78448987"
---
# <a name="create-the-view-ui"></a>Crear la vista (IU)

por [Mike Wasson](https://github.com/MikeWasson)

[Descargar proyecto completado](https://github.com/MikeWasson/BookService)

En esta sección, comenzará a definir el código HTML de la aplicación y agregará el enlace de datos entre el código HTML y el modelo de vista.

Abra el archivo views/home/index. cshtml. Reemplace todo el contenido de ese archivo por lo siguiente.

[!code-cshtml[Main](part-7/samples/sample1.cshtml)]

La mayoría de los elementos de `div` están ahí para el estilo [bootstrap](http://getbootstrap.com/) . Los elementos importantes son los que tienen atributos `data-bind`. Este atributo vincula el código HTML al modelo de vista.

Por ejemplo:

[!code-html[Main](part-7/samples/sample2.html)]

En este ejemplo, la &quot;`text`&quot; enlace hace que el elemento `<p>` muestre el valor de la propiedad `error` del modelo de vista. Recuerde que `error` se declaró como `ko.observable`:

[!code-javascript[Main](part-7/samples/sample3.js)]

Siempre que se asigna un nuevo valor a `error`, el knockout actualiza el texto del elemento `<p>`.

En el enlace de `foreach` se indica la cobertura para recorrer el contenido de la matriz de `books`. Para cada elemento de la matriz, el knockout crea un nuevo elemento &lt;Li&gt;. Los enlaces dentro del contexto de la `foreach` hacen referencia a las propiedades del elemento de la matriz. Por ejemplo:

[!code-html[Main](part-7/samples/sample4.html)]

Aquí, el enlace de `text` lee la propiedad Author de cada libro.

Si ejecuta la aplicación ahora, debería tener el siguiente aspecto:

![](part-7/_static/image1.png)

La lista de libros se carga de forma asincrónica, después de que se cargue la página. En este momento, los &quot;detalles&quot; vínculos no son funcionales. Agregaremos esta funcionalidad en la sección siguiente.

> [!div class="step-by-step"]
> [Anterior](part-6.md)
> [Siguiente](part-8.md)
