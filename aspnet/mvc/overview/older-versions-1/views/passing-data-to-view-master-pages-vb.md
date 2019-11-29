---
uid: mvc/overview/older-versions-1/views/passing-data-to-view-master-pages-vb
title: Pasar datos a las páginas maestras de vista (VB) | Microsoft Docs
author: microsoft
description: El objetivo de este tutorial es explicar cómo puede pasar datos de un controlador a una página maestra de vista. Examinamos dos estrategias para pasar datos a una vista m...
ms.author: riande
ms.date: 10/16/2008
ms.assetid: 37a1ebae-8773-408f-8645-d21da7ff9ae1
msc.legacyurl: /mvc/overview/older-versions-1/views/passing-data-to-view-master-pages-vb
msc.type: authoredcontent
ms.openlocfilehash: 9f768f47557adedc43cebfa2c092014bba5842de
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/28/2019
ms.locfileid: "74593729"
---
# <a name="passing-data-to-view-master-pages-vb"></a>Pasar datos a las páginas maestras de vista (VB)

por [Microsoft](https://github.com/microsoft)

[Descargar PDF](https://download.microsoft.com/download/e/f/3/ef3f2ff6-7424-48f7-bdaa-180ef64c3490/ASPNET_MVC_Tutorial_13_VB.pdf)

> El objetivo de este tutorial es explicar cómo puede pasar datos de un controlador a una página maestra de vista. Examinamos dos estrategias para pasar datos a una página maestra de vista. En primer lugar, se describe una solución sencilla que da lugar a una aplicación difícil de mantener. A continuación, examinamos una solución mucho mejor que requiere un poco más de trabajo inicial, pero da como resultado una aplicación mucho más fácil de mantener.

## <a name="passing-data-to-view-master-pages"></a>Pasar datos a las páginas maestras de vista

El objetivo de este tutorial es explicar cómo puede pasar datos de un controlador a una página maestra de vista. Examinamos dos estrategias para pasar datos a una página maestra de vista. En primer lugar, se describe una solución sencilla que da lugar a una aplicación difícil de mantener. A continuación, examinamos una solución mucho mejor que requiere un poco más de trabajo inicial, pero da como resultado una aplicación mucho más fácil de mantener.

### <a name="the-problem"></a>El problema

Imagine que está compilando una aplicación de base de datos de películas y desea mostrar la lista de categorías de películas en cada página de la aplicación (vea la ilustración 1). Imagine, además, que la lista de categorías de película se almacena en una tabla de base de datos. En ese caso, tendría sentido recuperar las categorías de la base de datos y representar la lista de categorías de películas en una página maestra de vista.

[![mostrar las categorías de películas en una página de vista maestra](passing-data-to-view-master-pages-vb/_static/image2.png)](passing-data-to-view-master-pages-vb/_static/image1.png)

**Figura 01**: visualización de las categorías de película en una página maestra de vista ([haga clic para ver la imagen de tamaño completo](passing-data-to-view-master-pages-vb/_static/image3.png))

Este es el problema. ¿Cómo se recupera la lista de categorías de películas en la página maestra? Es tentador llamar directamente a los métodos de las clases del modelo en la página maestra. En otras palabras, es tentador incluir el código para recuperar los datos de la base de datos directamente en la página maestra. Sin embargo, si se omiten los controladores MVC para tener acceso a la base de datos, se infringiría la separación limpia de preocupaciones que es una de las principales ventajas de la creación de una aplicación MVC.

En una aplicación MVC, desea que todas las interacciones entre las vistas de MVC y el modelo MVC se controlen mediante los controladores MVC. Esta separación de intereses da lugar a una aplicación más fácil de mantener, adaptable y comprobable.

En una aplicación MVC, todos los datos pasados a una vista, incluida una página maestra de vista, se deben pasar a una vista mediante una acción de controlador. Además, los datos se deben pasar aprovechando los datos de la vista. En el resto de este tutorial, se examinan dos métodos para pasar los datos de vista a una página maestra de vista.

### <a name="the-simple-solution"></a>La solución simple

Comencemos con la solución más sencilla para pasar datos de vista de un controlador a una página maestra de vista. La solución más sencilla consiste en pasar los datos de la vista de la página maestra en cada una de las acciones de controlador.

Considere el controlador en la lista 1. Expone dos acciones denominadas `Index()` y `Details()`. El método de acción `Index()` devuelve todas las películas de la tabla de base de datos de películas. El método de acción `Details()` devuelve todas las películas de una categoría de película determinada.

**Lista 1: `Controllers\HomeController.vb`**

[!code-vb[Main](passing-data-to-view-master-pages-vb/samples/sample1.vb)]

Observe que las acciones `Index()` y `Details()` agregan dos elementos para ver los datos. La acción `Index()` agrega dos claves: categorías y películas. La clave Categories representa la lista de categorías de película que muestra la página maestra de vista. La clave de películas representa la lista de películas que se muestra en la página vista de índice.

La acción `Details()` también agrega dos claves denominadas categorías y películas. La clave Categories, una vez más, representa la lista de categorías de película que muestra la página maestra de vista. La clave de películas representa la lista de películas de una categoría determinada que se muestra en la página vista de detalles (vea la figura 2).

[![la vista de detalles](passing-data-to-view-master-pages-vb/_static/image5.png)](passing-data-to-view-master-pages-vb/_static/image4.png)

**Figura 02**: vista de detalles ([haga clic para ver la imagen de tamaño completo](passing-data-to-view-master-pages-vb/_static/image6.png))

La vista de índice se encuentra en la lista 2. Simplemente recorre en iteración la lista de películas representada por el elemento Movies en View Data.

**Lista 2: `Views\Home\Index.aspx`**

[!code-aspx[Main](passing-data-to-view-master-pages-vb/samples/sample2.aspx)]

La página maestra de vista se incluye en la lista 3. La página ver maestro recorre en iteración y representa todas las categorías de películas representadas por el elemento categorías de ver datos.

**Lista 3: `Views\Shared\Site.master`**

[!code-aspx[Main](passing-data-to-view-master-pages-vb/samples/sample3.aspx)]

Todos los datos se pasan a la vista y a la página maestra de vista a través de los datos de la vista. Esta es la manera correcta de pasar los datos a la página maestra.

¿Cuál es el problema con esta solución? El problema es que esta solución infringe el principio seco (Una vez y solo una). Cada acción del controlador debe agregar la misma lista de categorías de películas para ver los datos. La existencia de código duplicado en la aplicación hace que la aplicación sea mucho más difícil de mantener, adaptar y modificar.

### <a name="the-good-solution"></a>La buena solución

En esta sección, examinaremos una solución alternativa y mejor para pasar datos de una acción de controlador a una página maestra de vista. En lugar de agregar las categorías de películas para la página maestra en cada una de las acciones de controlador, se agregan las categorías de películas a los datos de vista solo una vez. Todos los datos de la vista que usa la página maestra de vista se agregan a un controlador de aplicación.

La clase ApplicationController está incluida en la lista 4.

La clase ApplicationController está incluida en la lista 4.

**Lista 4: `Controllers\ApplicationController.vb`**

[!code-vb[Main](passing-data-to-view-master-pages-vb/samples/sample4.vb)]

Hay tres cosas que debe tener en cuenta sobre el controlador de aplicación en la lista 4. En primer lugar, observe que la clase se hereda de la clase base System. Web. Mvc. Controller. El controlador de aplicación es una clase de controlador.

En segundo lugar, tenga en cuenta que la clase de controlador de aplicación es una clase MustInherit. Una clase MustInherit es una clase que debe ser implementada por una clase concreta. Dado que el controlador de aplicación es una clase MustInherit, no se puede invocar directamente ningún método definido en la clase. Si intenta invocar la clase de aplicación directamente, aparecerá el mensaje de error no se encuentra el recurso.

En tercer lugar, observe que el controlador de la aplicación contiene un constructor que agrega la lista de categorías de películas para ver los datos. Cada clase de controlador que hereda del controlador de aplicación llama automáticamente al constructor del controlador de la aplicación. Siempre que llame a cualquier acción en cualquier controlador que herede del controlador de aplicación, las categorías de película se incluyen automáticamente en los datos de la vista.

El controlador de películas de la lista 5 hereda del controlador de aplicación.

**Lista 5: `Controllers\MoviesController.vb`**

[!code-vb[Main](passing-data-to-view-master-pages-vb/samples/sample5.vb)]

El controlador Movies, al igual que el controlador Home que se describe en la sección anterior, expone dos métodos de acción denominados `Index()` y `Details()`. Tenga en cuenta que la lista de categorías de película que se muestra en la página ver maestro no se agrega para ver los datos en el método `Index()` o `Details()`. Dado que el controlador de películas hereda del controlador de aplicación, se agrega la lista de categorías de películas para ver los datos automáticamente.

Tenga en cuenta que esta solución para agregar datos de vista para una página maestra de vista no infringe el principio seco (Una vez y solo una). El código para agregar la lista de categorías de películas para ver los datos se encuentra en una sola ubicación: el constructor del controlador de aplicación.

### <a name="summary"></a>Resumen

En este tutorial, se han analizado dos enfoques para pasar datos de vista de un controlador a una página maestra de vista. En primer lugar, examinamos un enfoque sencillo, pero difícil de mantener. En la primera sección, se describe cómo se pueden agregar datos de vista para una página maestra de vista en cada acción de controlador de la aplicación. Hemos concluido que se trata de un enfoque incorrecto porque infringe el principio seco (Una vez y solo una).

A continuación, examinamos una estrategia mucho mejor para agregar los datos que necesita una página maestra de vista para ver los datos. En lugar de agregar los datos de la vista en cada una de las acciones del controlador, se han agregado los datos de la vista solo una vez dentro de un controlador de aplicación. De este modo, puede evitar el código duplicado al pasar datos a una página maestra de vista en una aplicación ASP.NET MVC.

> [!div class="step-by-step"]
> [Anterior](creating-page-layouts-with-view-master-pages-vb.md)
