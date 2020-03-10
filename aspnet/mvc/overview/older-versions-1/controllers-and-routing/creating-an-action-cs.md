---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-an-action-cs
title: Crear una acción (C#) | Microsoft Docs
author: microsoft
description: Obtenga información sobre cómo agregar una nueva acción a un controlador MVC de ASP.NET. Obtenga información sobre los requisitos para que un método sea una acción.
ms.author: riande
ms.date: 03/02/2009
ms.assetid: cb33b28c-3025-4bd1-a1fa-eaa3af7bb56f
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-an-action-cs
msc.type: authoredcontent
ms.openlocfilehash: ebba935383819935ad85c95245666f4eaf6a0dca
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78470203"
---
# <a name="creating-an-action-c"></a>Crear una acción (C#)

por [Microsoft](https://github.com/microsoft)

> Obtenga información sobre cómo agregar una nueva acción a un controlador MVC de ASP.NET. Obtenga información sobre los requisitos para que un método sea una acción.

El objetivo de este tutorial es explicar cómo puede crear una nueva acción de controlador. Conocerá los requisitos de un método de acción. También aprenderá a evitar que un método se exponga como una acción.

## <a name="adding-an-action-to-a-controller"></a>Agregar una acción a un controlador

Agregue una nueva acción a un controlador agregando un nuevo método al controlador. Por ejemplo, el controlador de la lista 1 contiene una acción denominada index () y una acción denominada SayHello (). Ambos métodos se exponen como acciones.

**Lista 1-Controllers\HomeController.cs**

[!code-csharp[Main](creating-an-action-cs/samples/sample1.cs)]

Para que se exponga al universo como una acción, un método debe cumplir ciertos requisitos:

- El método debe ser público.
- El método no puede ser un método estático.
- El método no puede ser un método de extensión.
- El método no puede ser un constructor, un captador o un establecedor.
- El método no puede tener tipos genéricos abiertos.
- El método no es un método de la clase base del controlador.
- El método no puede contener parámetros **ref** ni **out** .

Observe que no hay restricciones en el tipo de valor devuelto de una acción de controlador. Una acción de controlador puede devolver una cadena, un valor DateTime, una instancia de la clase Random o void. El marco de MVC de ASP.NET convertirá cualquier tipo de valor devuelto que no sea un resultado de acción en una cadena y represente la cadena en el explorador.

Al agregar cualquier método que no infrinja estos requisitos en un controlador, el método se expone como una acción de controlador. Tenga cuidado aquí. Cualquier persona conectada a Internet puede invocar una acción de controlador. Por ejemplo, no cree una acción del controlador DeleteMyWebsite ().

## <a name="preventing-a-public-method-from-being-invoked"></a>Impedir que se invoque un método público

Si necesita crear un método público en una clase de controlador y no desea exponer el método como una acción de controlador, puede evitar que el método se invoque mediante el atributo [no Action]. Por ejemplo, el controlador de la lista 2 contiene un método público denominado CompanySecrets () que se decora con el atributo [nonaction].

**Lista 2-Controllers\WorkController.cs**

[!code-csharp[Main](creating-an-action-cs/samples/sample2.cs)]

Si intenta invocar la acción del controlador CompanySecrets () escribiendo/Work/CompanySecrets en la barra de direcciones del explorador, obtendrá el mensaje de error en la figura 1.

[![invocar un método que no es de acción](creating-an-action-cs/_static/image1.jpg)](creating-an-action-cs/_static/image1.png)

**Figura 01**: invocación de un método de no acción ([haga clic para ver la imagen de tamaño completo](creating-an-action-cs/_static/image2.png))

> [!div class="step-by-step"]
> [Anterior](creating-a-controller-cs.md)
> [Siguiente](asp-net-mvc-routing-overview-vb.md)
