---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-custom-routes-vb
title: Crear rutas personalizadas (VB) | Microsoft Docs
author: microsoft
description: Aprenda a agregar rutas personalizadas a una aplicación ASP.NET MVC. En este tutorial, aprenderá a modificar la tabla de rutas predeterminada en el archivo global. asax.
ms.author: riande
ms.date: 02/16/2009
ms.assetid: 6ac5758b-6199-42af-adcb-21954b864951
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-custom-routes-vb
msc.type: authoredcontent
ms.openlocfilehash: 22b44e9e575c9d404881a23ee735bb0c8b7109e1
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78486697"
---
# <a name="creating-custom-routes-vb"></a>Crear rutas personalizadas (VB)

por [Microsoft](https://github.com/microsoft)

> Aprenda a agregar rutas personalizadas a una aplicación ASP.NET MVC. En este tutorial, aprenderá a modificar la tabla de rutas predeterminada en el archivo global. asax.

En este tutorial, aprenderá a agregar una ruta personalizada a una aplicación ASP.NET MVC. Aprenderá a modificar la tabla de rutas predeterminada en el archivo global. asax con una ruta personalizada.

En las aplicaciones de ASP.NET MVC, la tabla de rutas predeterminada funcionará correctamente. Sin embargo, es posible que descubra que tiene necesidades de enrutamiento especializadas. En ese caso, puede crear una ruta personalizada.

Imagine, por ejemplo, que va a compilar una aplicación de blog. Es posible que desee controlar las solicitudes entrantes que tienen el siguiente aspecto:

/Archive/12-25-2009

Cuando un usuario escribe esta solicitud, desea devolver la entrada de blog correspondiente a la fecha 12/25/2009. Para controlar este tipo de solicitud, debe crear una ruta personalizada.

El archivo global. asax de la lista 1 contiene una nueva ruta personalizada, denominada blog, que controla las solicitudes que se parecen a la*fecha de entrada*/Archive/.

**Lista 1: global. asax (con ruta personalizada)**

[!code-vb[Main](creating-custom-routes-vb/samples/sample1.vb)]

Es importante el orden de las rutas que se agregan a la tabla de rutas. La nueva ruta de blog personalizada se agrega antes que la ruta predeterminada existente. Si invirtió el orden, la ruta predeterminada siempre se llamará en lugar de la ruta personalizada.

La ruta de blog personalizada coincide con cualquier solicitud que empiece por/Archive/. Por lo tanto, coincide con todas las direcciones URL siguientes:

- /Archive/12-25-2009

- /Archive/10-6-2004

- /Archive/apple

La ruta personalizada asigna la solicitud entrante a un controlador denominado Archive e invoca la acción entry (). Cuando se llama al método entry (), la fecha de entrada se pasa como un parámetro denominado entryDate.

Puede usar la ruta personalizada del blog con el controlador en la lista 2.

**Lista 2: ArchiveController. VB**

[!code-vb[Main](creating-custom-routes-vb/samples/sample2.vb)]

Observe que el método entry () de la lista 2 acepta un parámetro de tipo DateTime. El marco de MVC es lo suficientemente inteligente como para convertir automáticamente la fecha de entrada de la dirección URL en un valor DateTime. Si el parámetro de fecha de entrada de la dirección URL no se puede convertir en un valor DateTime, se genera un error (vea la figura 1).

**Figura 1: error al convertir el parámetro**

[![el cuadro de diálogo nuevo proyecto](creating-custom-routes-vb/_static/image1.jpg)](creating-custom-routes-vb/_static/image1.png)

**Figura 01**: error al convertir el parámetro ([haga clic para ver la imagen de tamaño completo](creating-custom-routes-vb/_static/image2.png))

## <a name="summary"></a>Resumen

El objetivo de este tutorial era mostrar cómo puede crear una ruta personalizada. Aprendió a agregar una ruta personalizada a la tabla de rutas en el archivo global. asax que representa las entradas de blog. Hemos explicado cómo asignar solicitudes de entradas de blog a un controlador denominado ArchiveController y una acción de controlador denominada entry ().

> [!div class="step-by-step"]
> [Anterior](asp-net-mvc-controller-overview-vb.md)
> [Siguiente](creating-a-route-constraint-vb.md)
