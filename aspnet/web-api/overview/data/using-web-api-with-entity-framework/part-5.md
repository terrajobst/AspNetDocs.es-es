---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-5
title: Crear objetos de Transferencia de datos (DTO) | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 06/16/2014
ms.assetid: 0fd07176-b74b-48f0-9fac-0f02e3ffa213
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-5
msc.type: authoredcontent
ms.openlocfilehash: fc0463420207eba764014b8ec7123c5150e38247
ms.sourcegitcommit: 84b1681d4e6253e30468c8df8a09fe03beea9309
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/02/2019
ms.locfileid: "73445760"
---
# <a name="create-data-transfer-objects-dtos"></a>Crear objetos de transferencia de datos (DTO)

por [Mike Wasson](https://github.com/MikeWasson)

[Descargar proyecto completado](https://github.com/MikeWasson/BookService)

En este momento, nuestra API Web expone las entidades de base de datos al cliente. El cliente recibe los datos que se asignan directamente a las tablas de base de datos. Sin embargo, no siempre es una buena idea. A veces, desea cambiar la forma de los datos que envía al cliente. Por ejemplo, es posible que quiera:

- Quitar referencias circulares (vea la sección anterior).
- Oculte determinadas propiedades que los clientes no deben ver.
- Omita algunas propiedades con el fin de reducir el tamaño de la carga.
- Acople los gráficos de objetos que contienen objetos anidados para que sean más convenientes para los clientes.
- Evite las vulnerabilidades de "exceso de publicaciones". (Consulte [validación de modelos](../../formats-and-model-binding/model-validation-in-aspnet-web-api.md) para obtener una explicación de la publicación en exceso).
- Desacople la capa de servicio del nivel de base de datos.

Para ello, puede definir un objeto de *transferencia de datos* (DTO). Un DTO es un objeto que define cómo se enviarán los datos a través de la red. Veamos cómo funciona con la entidad Book. En la carpeta modelos, agregue dos clases DTO:

[!code-csharp[Main](part-5/samples/sample1.cs)]

La clase `BookDetailDto` incluye todas las propiedades del modelo de libro, salvo que `AuthorName` es una cadena que contendrá el nombre del autor. La clase `BookDto` contiene un subconjunto de propiedades de `BookDetailDto`.

A continuación, reemplace los dos métodos GET de la clase `BooksController`, con las versiones que devuelven dto. Usaremos la instrucción **Select** de LINQ para convertir de entidades de libro en dto.

[!code-csharp[Main](part-5/samples/sample2.cs)]

Este es el SQL generado por el método New `GetBooks`. Puede ver que EF traduce la **selección** de LINQ en una instrucción SELECT de SQL.

[!code-sql[Main](part-5/samples/sample3.sql)]

Por último, modifique el método `PostBook` para devolver un valor DTO.

[!code-csharp[Main](part-5/samples/sample4.cs)]

> [!NOTE]
> En este tutorial, vamos a convertir a dto manualmente en el código. Otra opción es usar una biblioteca como [automapper](http://automapper.org/) que controla la conversión automáticamente.
> 
> [!div class="step-by-step"]
> [Anterior](part-4.md)
> [Siguiente](part-6.md)
