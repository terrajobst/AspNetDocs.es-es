---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-a-custom-route-constraint-cs
title: Crear una restricción de ruta personalizadaC#() | Microsoft Docs
author: StephenWalther
description: Stephen Walther muestra cómo puede crear una restricción de ruta personalizada. Implementamos una restricción personalizada simple que impide que una ruta coincida con...
ms.author: riande
ms.date: 02/16/2009
ms.assetid: a4f4bf4e-abcc-4650-8f43-527e48b52fe6
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-a-custom-route-constraint-cs
msc.type: authoredcontent
ms.openlocfilehash: 98d5839e3d2623665770ccc5689c28f9eb29c8f6
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78486829"
---
# <a name="creating-a-custom-route-constraint-c"></a>Crear una restricción de ruta personalizada (C#)

por [Stephen Walther](https://github.com/StephenWalther)

> Stephen Walther muestra cómo puede crear una restricción de ruta personalizada. Implementamos una restricción personalizada simple que impide que se haga coincidir una ruta cuando se realiza una solicitud de explorador desde un equipo remoto.

El objetivo de este tutorial es mostrar cómo se puede crear una restricción de ruta personalizada. Una restricción de ruta personalizada permite evitar que una ruta coincida a menos que coincida alguna condición personalizada.

En este tutorial, se crea una restricción de ruta localhost. La restricción de ruta localhost solo coincide con las solicitudes realizadas desde el equipo local. No se buscan coincidencias con las solicitudes remotas desde Internet.

Implemente una restricción de ruta personalizada implementando la interfaz IRouteConstraint. Se trata de una interfaz muy sencilla que describe un método único:

[!code-csharp[Main](creating-a-custom-route-constraint-cs/samples/sample1.cs)]

El método devuelve un valor booleano. Si devuelve false, la ruta asociada a la restricción no coincidirá con la solicitud del explorador.

La restricción localhost está incluida en la lista 1.

**Lista 1-LocalhostConstraint.cs**

[!code-csharp[Main](creating-a-custom-route-constraint-cs/samples/sample2.cs)]

La restricción de la lista 1 aprovecha la propiedad IsLocal expuesta por la clase HttpRequest. Esta propiedad devuelve true cuando la dirección IP de la solicitud es 127.0.0.1 o cuando la IP de la solicitud es la misma que la dirección IP del servidor.

Use una restricción personalizada en una ruta definida en el archivo global. asax. El archivo global. asax de la lista 2 usa la restricción localhost para evitar que alguien solicite una página de administración a menos que realice la solicitud desde el servidor local. Por ejemplo, se producirá un error en una solicitud de/Admin/DeleteAll cuando se realice desde un servidor remoto.

**Lista 2: global. asax**

[!code-csharp[Main](creating-a-custom-route-constraint-cs/samples/sample3.cs)]

La restricción localhost se usa en la definición de la ruta de administración. Esta ruta no coincidirá con una solicitud de explorador remoto. No obstante, tenga en cuentan que otras rutas definidas en global. asax pueden coincidir con la misma solicitud. Es importante comprender que una restricción impide que una ruta determinada coincida con una solicitud y no con todas las rutas definidas en el archivo global. asax.

Tenga en cuenta que la ruta predeterminada se ha comentado del archivo global. asax en la lista 2. Si incluye la ruta predeterminada, la ruta predeterminada coincidiría con las solicitudes del controlador de administración. En ese caso, los usuarios remotos pueden seguir invocando acciones del controlador de administración aunque sus solicitudes no coincidan con la ruta de administrador.

> [!div class="step-by-step"]
> [Anterior](creating-a-route-constraint-cs.md)
> [Siguiente](asp-net-mvc-controller-overview-vb.md)
