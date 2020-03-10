---
uid: mvc/overview/older-versions-1/controllers-and-routing/understanding-action-filters-vb
title: Descripción de los filtros de acción (VB) | Microsoft Docs
author: microsoft
description: El objetivo de este tutorial es explicar los filtros de acción. Un filtro de acción es un atributo que se puede aplicar a una acción de controlador, o a un controlador completo...
ms.author: riande
ms.date: 10/16/2008
ms.assetid: e83812f2-c53e-4a43-a7c1-d64c59ecf694
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/understanding-action-filters-vb
msc.type: authoredcontent
ms.openlocfilehash: 263658231ccaa7863508c691a3570bc00b9e8039
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78486589"
---
# <a name="understanding-action-filters-vb"></a>Descripción de los filtros de acción (VB)

por [Microsoft](https://github.com/microsoft)

[Descargar PDF](https://download.microsoft.com/download/e/f/3/ef3f2ff6-7424-48f7-bdaa-180ef64c3490/ASPNET_MVC_Tutorial_14_VB.pdf)

> El objetivo de este tutorial es explicar los filtros de acción. Un filtro de acción es un atributo que se puede aplicar a una acción de controlador (o a un controlador completo) que modifica el modo en que se ejecuta la acción.

## <a name="understanding-action-filters"></a>Descripción de los filtros de acción

El objetivo de este tutorial es explicar los filtros de acción. Un filtro de acción es un atributo que se puede aplicar a una acción de controlador (o a un controlador completo) que modifica el modo en que se ejecuta la acción. El marco de MVC de ASP.NET incluye varios filtros de acción:

- OutputCache: este filtro de acción almacena en caché el resultado de una acción de controlador durante un período de tiempo especificado.
- HandleError: este filtro de acción controla los errores que se producen cuando se ejecuta una acción de controlador.
- Autorizar: este filtro de acción le permite restringir el acceso a un usuario o rol determinado.

También puede crear sus propios filtros de acción personalizados. Por ejemplo, puede que desee crear un filtro de acción personalizado para implementar un sistema de autenticación personalizado. O bien, puede que desee crear un filtro de acción que modifique los datos de la vista devueltos por una acción del controlador.

En este tutorial, aprenderá a crear un filtro de acción desde el principio. Creamos un filtro de acción de registro que registra diferentes fases del procesamiento de una acción en la ventana de salida de Visual Studio.

### <a name="using-an-action-filter"></a>Usar un filtro de acción

Un filtro de acción es un atributo. Puede aplicar la mayoría de los filtros de acción a una acción de controlador individual o a un controlador completo.

Por ejemplo, el controlador de datos de la lista 1 expone una acción denominada `Index()` que devuelve la hora actual. Esta acción se decora con el `OutputCache` filtro de acción. Este filtro hace que el valor devuelto por la acción se almacene en caché durante 10 segundos.

**Lista 1: `Controllers\DataController.vb`**

[!code-vb[Main](understanding-action-filters-vb/samples/sample1.vb)]

Si invoca repetidamente la acción `Index()` escribiendo la dirección URL/Data/Index en la barra de direcciones del explorador y pulsando el botón de actualización varias veces, verá la misma hora durante 10 segundos. La salida de la acción `Index()` se almacena en caché durante 10 segundos (vea la figura 1).

[![la hora de almacenamiento en caché](understanding-action-filters-vb/_static/image2.png)](understanding-action-filters-vb/_static/image1.png)

**Figura 01**: hora de almacenamiento en caché ([haga clic para ver la imagen de tamaño completo](understanding-action-filters-vb/_static/image3.png))

En la enumeración 1, un filtro de acción único (el `OutputCache` filtro de acción) se aplica al método `Index()`. Si necesita, puede aplicar varios filtros de acción a la misma acción. Por ejemplo, puede que desee aplicar los filtros de acción `OutputCache` y `HandleError` a la misma acción.

En la lista 1, el filtro de acción de `OutputCache` se aplica a la acción de `Index()`. También podría aplicar este atributo a la propia clase `DataController`. En ese caso, el resultado devuelto por cualquier acción expuesta por el controlador se almacenaría en memoria caché durante 10 segundos.

### <a name="the-different-types-of-filters"></a>Los diferentes tipos de filtros

El marco de MVC de ASP.NET admite cuatro tipos diferentes de filtros:

1. Filtros de autorización: implementa el atributo `IAuthorizationFilter`.
2. Filtros de acción: implementa el atributo `IActionFilter`.
3. Filtros de resultados: implementa el atributo `IResultFilter`.
4. Filtros de excepciones: implementa el atributo `IExceptionFilter`.

Los filtros se ejecutan en el orden mostrado anteriormente. Por ejemplo, los filtros de autorización siempre se ejecutan antes de que los filtros de acción y los filtros de excepciones siempre se ejecuten después de cada otro tipo de filtro.

Los filtros de autorización se usan para implementar la autenticación y la autorización para las acciones del controlador. Por ejemplo, el filtro Authorize es un ejemplo de un filtro de autorización.

Los filtros de acción contienen lógica que se ejecuta antes y después de que se ejecute una acción de controlador. Puede usar un filtro de acción, por ejemplo, para modificar los datos de la vista que devuelve una acción de controlador.

Los filtros de resultados contienen lógica que se ejecuta antes y después de que se ejecute un resultado de la vista. Por ejemplo, puede que desee modificar un resultado de la vista justo antes de que la vista se represente en el explorador.

Los filtros de excepciones son el último tipo de filtro que se va a ejecutar. Puede usar un filtro de excepciones para controlar los errores generados por las acciones del controlador o los resultados de la acción del controlador. También puede usar filtros de excepciones para registrar errores.

Cada tipo de filtro se ejecuta en un orden determinado. Si desea controlar el orden en el que se ejecutan los filtros del mismo tipo, puede establecer la propiedad order de un filtro.

La clase base para todos los filtros de acción es la clase `System.Web.Mvc.FilterAttribute`. Si desea implementar un tipo determinado de filtro, debe crear una clase que herede de la clase de filtro base e implemente una o varias de las interfaces IAuthorizationFilter, IActionFilter, IResultFilter o ExceptionFilter.

### <a name="the-base-actionfilterattribute-class"></a>La clase base ActionFilterAttribute

Con el fin de facilitar la implementación de un filtro de acción personalizado, el marco de MVC de ASP.NET incluye una clase base `ActionFilterAttribute`. Esta clase implementa las interfaces `IActionFilter` y `IResultFilter` y hereda de la clase `Filter`.

La terminología aquí no es totalmente coherente. Técnicamente, una clase que hereda de la clase ActionFilterAttribute es un filtro de acción y un filtro de resultados. Sin embargo, en el sentido impreciso, el filtro de acción de Word se usa para hacer referencia a cualquier tipo de filtro en el marco de ASP.NET MVC.

La clase base ActionFilterAttribute tiene los siguientes métodos que puede invalidar:

- OnActionExecuting: se llama a este método antes de que se ejecute una acción de controlador.
- OnActionExecuted: se llama a este método después de que se ejecute una acción de controlador.
- OnResultExecuting: se llama a este método antes de que se ejecute un resultado de acción de controlador.
- OnResultExecuted: se llama a este método después de que se ejecute un resultado de acción de controlador.

En la siguiente sección, veremos cómo puede implementar cada uno de estos métodos diferentes.

### <a name="creating-a-log-action-filter"></a>Crear un filtro de acción de registro

Para ilustrar cómo puede crear un filtro de acción personalizado, vamos a crear un filtro de acción personalizado que registra las fases de procesamiento de una acción de controlador en la ventana de salida de Visual Studio. Nuestra `LogActionFilter` está incluida en la lista 2.

**Lista 2: `ActionFilters\LogActionFilter.vb`**

[!code-vb[Main](understanding-action-filters-vb/samples/sample2.vb)]

En la lista 2, los métodos `OnActionExecuting()`, `OnActionExecuted()`, `OnResultExecuting()`y `OnResultExecuted()` llaman al método `Log()`. El nombre del método y los datos de la ruta actual se pasan al método `Log()`. El método `Log()` escribe un mensaje en la ventana de salida de Visual Studio (vea la figura 2).

[![escribir en la ventana de salida de Visual Studio](understanding-action-filters-vb/_static/image5.png)](understanding-action-filters-vb/_static/image4.png)

**Figura 02**: escritura en la ventana de salida de Visual Studio ([haga clic para ver la imagen de tamaño completo](understanding-action-filters-vb/_static/image6.png))

El controlador Home de la lista 3 muestra cómo puede aplicar el filtro de acción de registro a una clase de controlador completa. Siempre que se invoca cualquiera de las acciones expuestas por el controlador de inicio, ya sea el método `Index()` o el método `About()`, las fases de procesamiento de la acción se registran en la ventana de salida de Visual Studio.

**Lista 3: `Controllers\HomeController.vb`**

[!code-vb[Main](understanding-action-filters-vb/samples/sample3.vb)]

### <a name="summary"></a>Resumen

En este tutorial, se presentaron los filtros de acción de ASP.NET MVC. Ha aprendido acerca de los cuatro tipos diferentes de filtros: filtros de autorización, filtros de acción, filtros de resultados y filtros de excepciones. También ha aprendido acerca de la clase base `ActionFilterAttribute`.

Por último, aprendió a implementar un filtro de acción simple. Hemos creado un filtro de acción de registro que registra las fases de procesamiento de una acción de controlador en la ventana de salida de Visual Studio.

> [!div class="step-by-step"]
> [Anterior](asp-net-mvc-routing-overview-vb.md)
> [Siguiente](improving-performance-with-output-caching-vb.md)
