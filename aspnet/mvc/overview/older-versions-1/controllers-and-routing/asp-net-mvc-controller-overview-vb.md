---
uid: mvc/overview/older-versions-1/controllers-and-routing/asp-net-mvc-controller-overview-vb
title: Información general sobre el controlador de MVC de ASP.NET (VB) | Microsoft Docs
author: StephenWalther
description: En este tutorial, Stephen Walther presenta los controladores de ASP.NET MVC. Aprenderá a crear nuevos controladores y a devolver distintos tipos de acciones...
ms.author: riande
ms.date: 02/16/2008
ms.assetid: 94c3e5d9-a904-445e-a34e-d92fd1ca108a
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/asp-net-mvc-controller-overview-vb
msc.type: authoredcontent
ms.openlocfilehash: f19e7dd7fc025de2e0c387db898d36623e790e6a
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78486925"
---
# <a name="aspnet-mvc-controller-overview-vb"></a>Información general sobre el controlador de ASP.NET MVC (VB)

por [Stephen Walther](https://github.com/StephenWalther)

> En este tutorial, Stephen Walther presenta los controladores de ASP.NET MVC. Aprenderá a crear nuevos controladores y a devolver distintos tipos de resultados de acciones.

En este tutorial se explora el tema de los controladores de ASP.NET MVC, las acciones de controlador y los resultados de acciones. Después de completar este tutorial, comprenderá cómo se usan los controladores para controlar la manera en que un visitante interactúa con un sitio web de ASP.NET MVC.

## <a name="understanding-controllers"></a>Descripción de los controladores

Los controladores de MVC son responsables de responder a las solicitudes realizadas en un sitio web de ASP.NET MVC. Cada solicitud del explorador se asigna a un controlador determinado. Por ejemplo, Imagine que escribe la siguiente dirección URL en la barra de direcciones del explorador:

`http://localhost/Product/Index/3`

En este caso, se invoca un controlador denominado ProductController. ProductController es responsable de generar la respuesta a la solicitud del explorador. Por ejemplo, el controlador podría devolver una vista determinada al explorador o el controlador podría redirigir al usuario a otro controlador.

La lista 1 contiene un controlador simple denominado ProductController.

**Listing1 - Controllers\ProductController.vb**

[!code-vb[Main](asp-net-mvc-controller-overview-vb/samples/sample1.vb)]

Como puede ver en la lista 1, un controlador es simplemente una clase (un Visual Basic .NET o C# una clase). Un controlador es una clase que se deriva de la clase base System. Web. Mvc. Controller. Dado que un controlador hereda de esta clase base, un controlador hereda varios métodos útiles de forma gratuita (se describen estos métodos en un momento).

## <a name="understanding-controller-actions"></a>Descripción de las acciones de controlador

Un controlador expone las acciones del controlador. Una acción es un método en un controlador que se llama cuando se escribe una dirección URL determinada en la barra de direcciones del explorador. Por ejemplo, Imagine que realiza una solicitud para la siguiente dirección URL:

`http://localhost/Product/Index/3`

En este caso, se llama al método index () en la clase ProductController. El método index () es un ejemplo de una acción de controlador.

Una acción de controlador debe ser un método público de una clase de controlador. De forma predeterminada, los métodos de Visual Basic.NET son métodos públicos. Tenga en consideración que cualquier método público que agregue a una clase de controlador se expone automáticamente como una acción de controlador (debe tener cuidado con esto, ya que cualquier persona del universo puede invocar una acción de controlador simplemente escribiendo la dirección URL correcta en una barra de direcciones del explorador).

Hay algunos requisitos adicionales que debe cumplir una acción de controlador. Un método utilizado como una acción de controlador no se puede sobrecargar. Además, una acción de controlador no puede ser un método estático. Aparte de eso, puede usar prácticamente cualquier método como una acción de controlador.

## <a name="understanding-action-results"></a>Descripción de los resultados de la acción

Una acción de controlador devuelve algo llamado *resultado de acción*. Un resultado de acción es lo que devuelve una acción del controlador en respuesta a una solicitud del explorador.

El marco de MVC de ASP.NET admite varios tipos de resultados de acciones, entre los que se incluyen:

1. ViewResult: representa HTML y marcado.
2. EmptyResult: no representa ningún resultado.
3. RedirectResult: representa una redirección a una nueva dirección URL.
4. JsonResult: representa un resultado notación de objetos JavaScript que se puede usar en una aplicación AJAX.
5. JavaScriptResult: representa un script de JavaScript.
6. ContentResult: representa un resultado de texto.
7. FileContentResult: representa un archivo descargable (con el contenido binario).
8. FilePathResult: representa un archivo descargable (con una ruta de acceso).
9. FileStreamResult: representa un archivo descargable (con un flujo de archivo).

Todos estos resultados de acción heredan de la clase base ActionResult.

En la mayoría de los casos, una acción de controlador devuelve un ViewResult. Por ejemplo, la acción del controlador de índice de la lista 2 devuelve un ViewResult.

**Lista 2-Controllers\BookController.vb**

[!code-vb[Main](asp-net-mvc-controller-overview-vb/samples/sample2.vb)]

Cuando una acción devuelve un ViewResult, se devuelve HTML al explorador. El método index () de la lista 2 devuelve una vista denominada index al explorador.

Tenga en cuenta que la acción index () de la lista 2 no devuelve ViewResult (). En su lugar, se llama al método View () de la clase base Controller. Normalmente, no se devuelve directamente un resultado de acción. En su lugar, se llama a uno de los métodos siguientes de la clase base del controlador:

1. View: devuelve el resultado de una acción de ViewResult.
2. Redirect: devuelve el resultado de una acción de RedirectResult.
3. RedirectToAction: devuelve el resultado de una acción RedirectToRouteResult.
4. RedirectToRoute: devuelve el resultado de una acción RedirectToRouteResult.
5. JSON: devuelve el resultado de una acción de JsonResult.
6. JavaScriptResult: devuelve un JavaScriptResult.
7. Contenido: devuelve el resultado de una acción de ContentResult.
8. Archivo: devuelve FileContentResult, FilePathResult o FileStreamResult, dependiendo de los parámetros pasados al método.

Por lo tanto, si desea devolver una vista al explorador, llame al método View (). Si desea redirigir al usuario de una acción de controlador a otra, llame al método RedirectToAction (). Por ejemplo, la acción Details () de la lista 3 muestra una vista o redirige al usuario a la acción index (), dependiendo de si el parámetro ID tiene un valor.

**Lista 3: CustomerController. VB**

[!code-vb[Main](asp-net-mvc-controller-overview-vb/samples/sample3.vb)]

El resultado de la acción ContentResult es especial. Puede usar el resultado de la acción ContentResult para devolver el resultado de una acción como texto sin formato. Por ejemplo, el método index () de la lista 4 devuelve un mensaje como texto sin formato y no como HTML.

**Lista 4: Controllers\StatusController.vb**

> StatusController
> 
> 
> System. Web. Mvc. Controller

[!code-vb[Main](asp-net-mvc-controller-overview-vb/samples/sample4.vb)]

Cuando se invoca la acción StatusController. index (), no se devuelve una vista. En su lugar, el texto sin formato "Hola mundo!" se devuelve al explorador.

Si una acción de controlador devuelve un resultado que no es un resultado de acción (por ejemplo, una fecha o un entero), el resultado se ajusta automáticamente en un ContentResult. Por ejemplo, cuando se invoca la acción index () de WorkController en la lista 5, la fecha se devuelve automáticamente como ContentResult.

**Lista 5-WorkController. VB**

[!code-vb[Main](asp-net-mvc-controller-overview-vb/samples/sample5.vb)]

La acción index () de la lista 5 devuelve un objeto DateTime. El marco de MVC de ASP.NET convierte el objeto DateTime en una cadena y ajusta el valor DateTime en un ContentResult automáticamente. El explorador recibe la fecha y la hora como texto sin formato.

## <a name="summary"></a>Resumen

El propósito de este tutorial fue presentar los conceptos de los controladores de ASP.NET MVC, las acciones de controlador y los resultados de acciones de controlador. En la primera sección, ha aprendido a agregar nuevos controladores a un proyecto de ASP.NET MVC. A continuación, ha aprendido cómo los métodos públicos de un controlador se exponen al universo como acciones de controlador. Por último, analizamos los diferentes tipos de resultados de acción que se pueden devolver desde una acción de controlador. En concreto, hemos explicado cómo devolver ViewResult, RedirectToActionResult y ContentResult desde una acción de controlador.

> [!div class="step-by-step"]
> [Anterior](creating-a-custom-route-constraint-cs.md)
> [Siguiente](creating-custom-routes-vb.md)
