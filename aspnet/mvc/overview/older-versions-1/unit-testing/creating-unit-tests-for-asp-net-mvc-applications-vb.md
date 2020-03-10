---
uid: mvc/overview/older-versions-1/unit-testing/creating-unit-tests-for-asp-net-mvc-applications-vb
title: Crear pruebas unitarias para aplicaciones ASP.NET MVC (VB) | Microsoft Docs
author: StephenWalther
description: Obtenga información sobre cómo crear pruebas unitarias para las acciones de controlador. En este tutorial, Stephen Walther muestra cómo probar si una acción del controlador devuelve una parte...
ms.author: riande
ms.date: 08/19/2008
ms.assetid: eb35710d-1d99-44ac-b61f-e50af8cb328a
msc.legacyurl: /mvc/overview/older-versions-1/unit-testing/creating-unit-tests-for-asp-net-mvc-applications-vb
msc.type: authoredcontent
ms.openlocfilehash: 643faddaf6f9cd075131e8e5a9cccb303e355ceb
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78435385"
---
# <a name="creating-unit-tests-for-aspnet-mvc-applications-vb"></a>Crear pruebas unitarias para aplicaciones de ASP.NET MVC (VB)

por [Stephen Walther](https://github.com/StephenWalther)

[Descargar PDF](https://download.microsoft.com/download/8/4/8/84843d8d-1575-426c-bcb5-9d0c42e51416/ASPNET_MVC_Tutorial_07_VB.pdf)

> Obtenga información sobre cómo crear pruebas unitarias para las acciones de controlador. En este tutorial, Stephen Walther muestra cómo probar si una acción de controlador devuelve una vista determinada, devuelve un conjunto de datos determinado o devuelve un tipo diferente de resultado de acción.

El objetivo de este tutorial es mostrar cómo se pueden escribir pruebas unitarias para los controladores en las aplicaciones ASP.NET MVC. Se explica cómo crear tres tipos diferentes de pruebas unitarias. Aprenderá a probar la vista devuelta por una acción del controlador, a probar los datos de la vista devueltos por una acción del controlador y a probar si una acción del controlador redirige o no a una segunda acción del controlador.

## <a name="creating-the-controller-under-test"></a>Creando el controlador sometido a prueba

Comencemos por crear el controlador que se va a probar. El controlador, denominado el `ProductController`, se incluye en la lista 1.

**Lista 1: `ProductController.vb`**

[!code-vb[Main](creating-unit-tests-for-asp-net-mvc-applications-vb/samples/sample1.vb)]

El `ProductController` contiene dos métodos de acción denominados `Index()` y `Details()`. Ambos métodos de acción devuelven una vista. Tenga en cuenta que la acción `Details()` acepta un parámetro denominado ID.

## <a name="testing-the-view-returned-by-a-controller"></a>Probar la vista devuelta por un controlador

Imagine que queremos probar si el `ProductController` devuelve o no la vista correcta. Queremos asegurarnos de que, cuando se invoca la acción `ProductController.Details()`, se devuelve la vista de detalles. La clase de prueba de la lista 2 contiene una prueba unitaria para probar la vista devuelta por la acción `ProductController.Details()`.

**Lista 2: `ProductControllerTest.vb`**

[!code-vb[Main](creating-unit-tests-for-asp-net-mvc-applications-vb/samples/sample2.vb)]

La clase de la lista 2 incluye un método de prueba denominado `TestDetailsView()`. Este método contiene tres líneas de código. La primera línea de código crea una nueva instancia de la clase `ProductController`. La segunda línea de código invoca el método de acción `Details()` del controlador. Por último, la última línea de código comprueba si la vista devuelta por la `Details()` acción es la vista de detalles.

La propiedad `ViewResult.ViewName` representa el nombre de la vista devuelta por un controlador. Una advertencia importante sobre la prueba de esta propiedad. Un controlador puede devolver una vista de dos maneras. Un controlador puede devolver explícitamente una vista similar a la siguiente:

[!code-vb[Main](creating-unit-tests-for-asp-net-mvc-applications-vb/samples/sample3.vb)]

Como alternativa, el nombre de la vista se puede inferir a partir del nombre de la acción del controlador como se muestra a continuación:

[!code-vb[Main](creating-unit-tests-for-asp-net-mvc-applications-vb/samples/sample4.vb)]

Esta acción del controlador también devuelve una vista denominada `Details`. Sin embargo, el nombre de la vista se deduce del nombre de la acción. Si desea probar el nombre de la vista, debe devolver explícitamente el nombre de la vista desde la acción del controlador.

Puede ejecutar la prueba unitaria en la lista 2 escribiendo la combinación **de teclas Ctrl-R,** o haciendo clic en el botón **ejecutar todas las pruebas de la solución** (consulte la figura 1). Si la prueba se supera, verá la Resultados de pruebas ventana en la figura 2.

[![ejecutar todas las pruebas de la solución](creating-unit-tests-for-asp-net-mvc-applications-vb/_static/image2.png)](creating-unit-tests-for-asp-net-mvc-applications-vb/_static/image1.png)

**Figura 01**: ejecución de todas las pruebas de la solución ([haga clic para ver la imagen de tamaño completo](creating-unit-tests-for-asp-net-mvc-applications-vb/_static/image3.png))

[![correcto.](creating-unit-tests-for-asp-net-mvc-applications-vb/_static/image5.png)](creating-unit-tests-for-asp-net-mvc-applications-vb/_static/image4.png)

**Figura 02**: correcto. ([Haga clic para ver la imagen de tamaño completo](creating-unit-tests-for-asp-net-mvc-applications-vb/_static/image6.png))

## <a name="testing-the-view-data-returned-by-a-controller"></a>Probar los datos de vista devueltos por un controlador

Un controlador de MVC pasa datos a una vista mediante algo llamado *`View Data`* . Por ejemplo, Imagine que desea mostrar los detalles de un producto determinado al invocar la acción `ProductController Details()`. En ese caso, puede crear una instancia de una `Product` clase (definida en el modelo) y pasar la instancia a la vista de `Details` aprovechando las ventajas de `View Data`.

Los `ProductController` modificados de la lista 3 incluyen una acción de `Details()` actualizada que devuelve un producto.

**Lista 3: `ProductController.vb`**

[!code-vb[Main](creating-unit-tests-for-asp-net-mvc-applications-vb/samples/sample5.vb)]

En primer lugar, la acción de `Details()` crea una instancia nueva de la clase `Product` que representa un equipo portátil. A continuación, la instancia de la clase `Product` se pasa como segundo parámetro al método `View()`.

Puede escribir pruebas unitarias para comprobar si los datos esperados se encuentran en los datos de la vista. La prueba unitaria de la lista 4 comprueba si se devuelve o no un producto que representa un equipo portátil cuando se llama al método de acción `ProductController Details()`.

**Lista 4: `ProductControllerTest.vb`**

[!code-vb[Main](creating-unit-tests-for-asp-net-mvc-applications-vb/samples/sample6.vb)]

En la lista 4, el método `TestDetailsView()` prueba los datos de vista devueltos invocando el método `Details()`. El `ViewData` se expone como una propiedad en el `ViewResult` devuelto al invocar el método `Details()`. La propiedad `ViewData.Model` contiene el producto que se pasa a la vista. La prueba simplemente comprueba que el producto contenido en los datos de la vista tiene el nombre Laptop.

## <a name="testing-the-action-result-returned-by-a-controller"></a>Probar el resultado de la acción devuelto por un controlador

Una acción de controlador más compleja podría devolver distintos tipos de resultados de acción en función de los valores de los parámetros pasados a la acción del controlador. Una acción del controlador puede devolver una variedad de tipos de resultados de acción, incluidos un `ViewResult`, `RedirectToRouteResult`o `JsonResult`.

Por ejemplo, la acción de `Details()` modificada de la lista 5 devuelve la vista de `Details` cuando se pasa un ID. de producto válido a la acción. Si pasa un ID. de producto no válido (un identificador con un valor inferior a 1), se le redirigirá a la acción `Index()`.

**Lista 5: `ProductController.vb`**

[!code-vb[Main](creating-unit-tests-for-asp-net-mvc-applications-vb/samples/sample7.vb)]

Puede probar el comportamiento de la acción `Details()` con la prueba unitaria en la lista 6. La prueba unitaria de la lista 6 comprueba que se le redirige a la `Index` vista cuando se pasa un identificador con el valor-1 al método `Details()`.

**Lista 6: `ProductControllerTest.vb`**

[!code-vb[Main](creating-unit-tests-for-asp-net-mvc-applications-vb/samples/sample8.vb)]

Cuando se llama al método `RedirectToAction()` en una acción del controlador, la acción del controlador devuelve un `RedirectToRouteResult`. La prueba comprueba si el `RedirectToRouteResult` redirigirá al usuario a una acción de controlador llamada `Index`.

## <a name="summary"></a>Resumen

En este tutorial, aprendió a crear pruebas unitarias para las acciones de controlador de MVC. En primer lugar, aprendió a comprobar si la vista correcta es devuelta por una acción del controlador. Aprendió a utilizar la propiedad `ViewResult.ViewName` para comprobar el nombre de una vista.

A continuación, hemos examinado cómo puede probar el contenido de `View Data`. Aprendió a comprobar si el producto correcto se devolvió en `View Data` después de llamar a una acción de controlador.

Por último, se explica cómo puede probar si se devuelven diferentes tipos de resultados de acciones desde una acción de controlador. Aprendió a probar si un controlador devuelve un `ViewResult` o un `RedirectToRouteResult`.

> [!div class="step-by-step"]
> [Anterior](creating-unit-tests-for-asp-net-mvc-applications-cs.md)
