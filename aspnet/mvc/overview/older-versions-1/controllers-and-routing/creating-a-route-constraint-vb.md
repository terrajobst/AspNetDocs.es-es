---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-a-route-constraint-vb
title: Crear una restricción de ruta (VB) | Microsoft Docs
author: StephenWalther
description: En este tutorial, Stephen Walther muestra cómo puede controlar cómo las solicitudes del explorador coinciden con las rutas mediante la creación de restricciones de ruta con expresiones regulares.
ms.author: riande
ms.date: 02/16/2009
ms.assetid: b7cce113-c82c-45bf-b97b-357e5d9f7f56
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-a-route-constraint-vb
msc.type: authoredcontent
ms.openlocfilehash: 205742dd8f866c8828008c8aac7ab3f98b173ceb
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78486775"
---
# <a name="creating-a-route-constraint-vb"></a>Crear una restricción de ruta (VB)

por [Stephen Walther](https://github.com/StephenWalther)

> En este tutorial, Stephen Walther muestra cómo puede controlar cómo las solicitudes del explorador coinciden con las rutas mediante la creación de restricciones de ruta con expresiones regulares.

Las restricciones de ruta se usan para restringir las solicitudes del explorador que coinciden con una ruta determinada. Puede usar una expresión regular para especificar una restricción de ruta.

Por ejemplo, Imagine que ha definido la ruta en la lista 1 en el archivo global. asax.

**Lista 1: global. asax. VB**

[!code-vb[Main](creating-a-route-constraint-vb/samples/sample1.vb)]

La lista 1 contiene una ruta denominada product. Puede usar la ruta del producto para asignar solicitudes del explorador al ProductController incluido en la lista 2.

**Lista 2-Controllers\ProductController.vb**

[!code-vb[Main](creating-a-route-constraint-vb/samples/sample2.vb)]

Tenga en cuenta que la acción Details () expuesta por el controlador del producto acepta un único parámetro denominado productId. Este parámetro es un parámetro entero.

La ruta definida en la enumeración 1 coincidirá con cualquiera de las siguientes direcciones URL:

- /Product/23
- /Product/7

Desafortunadamente, la ruta también coincidirá con las siguientes direcciones URL:

- /Product/blah
- /Product/apple

Dado que la acción Details () espera un parámetro de entero, la realización de una solicitud que contenga algo distinto de un valor entero producirá un error. Por ejemplo, si escribe la dirección URL/Product/Apple en el explorador, se mostrará la página de error en la figura 1.

[![el cuadro de diálogo nuevo proyecto](creating-a-route-constraint-vb/_static/image1.jpg)](creating-a-route-constraint-vb/_static/image1.png)

**Figura 01**: visualización de una página de explosión ([haga clic para ver la imagen de tamaño completo](creating-a-route-constraint-vb/_static/image2.png))

Lo que realmente desea hacer es que solo coincida con las direcciones URL que contengan un número entero de productId adecuado. Puede usar una restricción al definir una ruta para restringir las direcciones URL que coinciden con la ruta. La ruta del producto modificada de la lista 3 contiene una restricción de expresión regular que solo coincide con enteros.

**Lista 3: global. asax. VB**

[!code-vb[Main](creating-a-route-constraint-vb/samples/sample3.vb)]

La expresión regular \d + coincide con uno o varios enteros. Esta restricción hace que la ruta del producto coincida con las siguientes direcciones URL:

- /Product/3
- /Product/8999

Pero no las direcciones URL siguientes:

- /Product/apple
- /Product

Estas solicitudes del explorador se controlarán mediante otra ruta o, si no hay ninguna ruta coincidente, se devolverá un error *de recurso no encontrado* .

> [!div class="step-by-step"]
> [Anterior](creating-custom-routes-vb.md)
> [Siguiente](creating-a-custom-route-constraint-vb.md)
