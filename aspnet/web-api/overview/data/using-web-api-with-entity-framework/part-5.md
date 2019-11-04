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
# <a name="create-data-transfer-objects-dtos"></a><span data-ttu-id="b44ed-102">Crear objetos de transferencia de datos (DTO)</span><span class="sxs-lookup"><span data-stu-id="b44ed-102">Create Data Transfer Objects (DTOs)</span></span>

<span data-ttu-id="b44ed-103">por [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="b44ed-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="b44ed-104">Descargar proyecto completado</span><span class="sxs-lookup"><span data-stu-id="b44ed-104">Download Completed Project</span></span>](https://github.com/MikeWasson/BookService)

<span data-ttu-id="b44ed-105">En este momento, nuestra API Web expone las entidades de base de datos al cliente.</span><span class="sxs-lookup"><span data-stu-id="b44ed-105">Right now, our web API exposes the database entities to the client.</span></span> <span data-ttu-id="b44ed-106">El cliente recibe los datos que se asignan directamente a las tablas de base de datos.</span><span class="sxs-lookup"><span data-stu-id="b44ed-106">The client receives data that maps directly to your database tables.</span></span> <span data-ttu-id="b44ed-107">Sin embargo, no siempre es una buena idea.</span><span class="sxs-lookup"><span data-stu-id="b44ed-107">However, that's not always a good idea.</span></span> <span data-ttu-id="b44ed-108">A veces, desea cambiar la forma de los datos que envía al cliente.</span><span class="sxs-lookup"><span data-stu-id="b44ed-108">Sometimes you want to change the shape of the data that you send to client.</span></span> <span data-ttu-id="b44ed-109">Por ejemplo, puedes:</span><span class="sxs-lookup"><span data-stu-id="b44ed-109">For example, you might want to:</span></span>

- <span data-ttu-id="b44ed-110">Quitar referencias circulares (vea la sección anterior).</span><span class="sxs-lookup"><span data-stu-id="b44ed-110">Remove circular references (see previous section).</span></span>
- <span data-ttu-id="b44ed-111">Oculte determinadas propiedades que los clientes no deben ver.</span><span class="sxs-lookup"><span data-stu-id="b44ed-111">Hide particular properties that clients are not supposed to view.</span></span>
- <span data-ttu-id="b44ed-112">Omita algunas propiedades con el fin de reducir el tamaño de la carga.</span><span class="sxs-lookup"><span data-stu-id="b44ed-112">Omit some properties in order to reduce payload size.</span></span>
- <span data-ttu-id="b44ed-113">Acople los gráficos de objetos que contienen objetos anidados para que sean más convenientes para los clientes.</span><span class="sxs-lookup"><span data-stu-id="b44ed-113">Flatten object graphs that contain nested objects, to make them more convenient for clients.</span></span>
- <span data-ttu-id="b44ed-114">Evite las vulnerabilidades de "exceso de publicaciones".</span><span class="sxs-lookup"><span data-stu-id="b44ed-114">Avoid "over-posting" vulnerabilities.</span></span> <span data-ttu-id="b44ed-115">(Consulte [validación de modelos](../../formats-and-model-binding/model-validation-in-aspnet-web-api.md) para obtener una explicación de la publicación en exceso).</span><span class="sxs-lookup"><span data-stu-id="b44ed-115">(See [Model Validation](../../formats-and-model-binding/model-validation-in-aspnet-web-api.md) for a discussion of over-posting.)</span></span>
- <span data-ttu-id="b44ed-116">Desacople la capa de servicio del nivel de base de datos.</span><span class="sxs-lookup"><span data-stu-id="b44ed-116">Decouple your service layer from your database layer.</span></span>

<span data-ttu-id="b44ed-117">Para ello, puede definir un objeto de *transferencia de datos* (DTO).</span><span class="sxs-lookup"><span data-stu-id="b44ed-117">To accomplish this, you can define a *data transfer object* (DTO).</span></span> <span data-ttu-id="b44ed-118">Un DTO es un objeto que define cómo se enviarán los datos a través de la red.</span><span class="sxs-lookup"><span data-stu-id="b44ed-118">A DTO is an object that defines how the data will be sent over the network.</span></span> <span data-ttu-id="b44ed-119">Veamos cómo funciona con la entidad Book.</span><span class="sxs-lookup"><span data-stu-id="b44ed-119">Let's see how that works with the Book entity.</span></span> <span data-ttu-id="b44ed-120">En la carpeta modelos, agregue dos clases DTO:</span><span class="sxs-lookup"><span data-stu-id="b44ed-120">In the Models folder, add two DTO classes:</span></span>

[!code-csharp[Main](part-5/samples/sample1.cs)]

<span data-ttu-id="b44ed-121">La clase `BookDetailDto` incluye todas las propiedades del modelo de libro, salvo que `AuthorName` es una cadena que contendrá el nombre del autor.</span><span class="sxs-lookup"><span data-stu-id="b44ed-121">The `BookDetailDto` class includes all of the properties from the Book model, except that `AuthorName` is a string that will hold the author name.</span></span> <span data-ttu-id="b44ed-122">La clase `BookDto` contiene un subconjunto de propiedades de `BookDetailDto`.</span><span class="sxs-lookup"><span data-stu-id="b44ed-122">The `BookDto` class contains a subset of properties from `BookDetailDto`.</span></span>

<span data-ttu-id="b44ed-123">A continuación, reemplace los dos métodos GET de la clase `BooksController`, con las versiones que devuelven dto.</span><span class="sxs-lookup"><span data-stu-id="b44ed-123">Next, replace the two GET methods in the `BooksController` class, with versions that return DTOs.</span></span> <span data-ttu-id="b44ed-124">Usaremos la instrucción **Select** de LINQ para convertir de entidades de libro en dto.</span><span class="sxs-lookup"><span data-stu-id="b44ed-124">We'll use the LINQ **Select** statement to convert from Book entities into DTOs.</span></span>

[!code-csharp[Main](part-5/samples/sample2.cs)]

<span data-ttu-id="b44ed-125">Este es el SQL generado por el método New `GetBooks`.</span><span class="sxs-lookup"><span data-stu-id="b44ed-125">Here is the SQL generated by the new `GetBooks` method.</span></span> <span data-ttu-id="b44ed-126">Puede ver que EF traduce la **selección** de LINQ en una instrucción SELECT de SQL.</span><span class="sxs-lookup"><span data-stu-id="b44ed-126">You can see that EF translates the LINQ **Select** into a SQL SELECT statement.</span></span>

[!code-sql[Main](part-5/samples/sample3.sql)]

<span data-ttu-id="b44ed-127">Por último, modifique el método `PostBook` para devolver un valor DTO.</span><span class="sxs-lookup"><span data-stu-id="b44ed-127">Finally, modify the `PostBook` method to return a DTO.</span></span>

[!code-csharp[Main](part-5/samples/sample4.cs)]

> [!NOTE]
> <span data-ttu-id="b44ed-128">En este tutorial, vamos a convertir a dto manualmente en el código.</span><span class="sxs-lookup"><span data-stu-id="b44ed-128">In this tutorial, we're converting to DTOs manually in code.</span></span> <span data-ttu-id="b44ed-129">Otra opción es usar una biblioteca como [automapper](http://automapper.org/) que controla la conversión automáticamente.</span><span class="sxs-lookup"><span data-stu-id="b44ed-129">Another option is to use a library like [AutoMapper](http://automapper.org/) that handles the conversion automatically.</span></span>
> 
> [!div class="step-by-step"]
> <span data-ttu-id="b44ed-130">[Anterior](part-4.md)
> [Siguiente](part-6.md)</span><span class="sxs-lookup"><span data-stu-id="b44ed-130">[Previous](part-4.md)
[Next](part-6.md)</span></span>
