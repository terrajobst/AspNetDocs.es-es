---
ms.openlocfilehash: 934db70fe49ba9a5330b25596dc694e0ac50c04e
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/01/2019
ms.locfileid: "57037902"
---
<span data-ttu-id="cb6f9-101">Por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="cb6f9-101">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="cb6f9-102">En esta sección, agregará las clases para administrar películas en una base de datos.</span><span class="sxs-lookup"><span data-stu-id="cb6f9-102">In this section, you add classes for managing movies in a database.</span></span> <span data-ttu-id="cb6f9-103">Estas clases se usan con [Entity Framework Core](/ef/core) (EF Core) para trabajar con una base de datos.</span><span class="sxs-lookup"><span data-stu-id="cb6f9-103">You use these classes with [Entity Framework Core](/ef/core) (EF Core) to work with a database.</span></span> <span data-ttu-id="cb6f9-104">EF Core es un marco de trabajo de asignación relacional de objetos (ORM) que simplifica el código de acceso de datos que se debe escribir.</span><span class="sxs-lookup"><span data-stu-id="cb6f9-104">EF Core is an object-relational mapping (ORM) framework that simplifies the data access code that you have to write.</span></span>

<span data-ttu-id="cb6f9-105">Las clases de modelo que se crean se conocen como clases POCO (del inglés "plain-old CLR objects", objetos CLR antiguos sin formato) porque no tienen ninguna dependencia de EF Core.</span><span class="sxs-lookup"><span data-stu-id="cb6f9-105">The model classes you create are known as POCO classes (from "plain-old CLR objects") because they don't have any dependency on EF Core.</span></span> <span data-ttu-id="cb6f9-106">Definen las propiedades de los datos que se almacenan en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="cb6f9-106">They define the properties of the data that are stored in the database.</span></span>

<span data-ttu-id="cb6f9-107">En este tutorial, se escriben primero las clases del modelo y EF Core crea la base de datos.</span><span class="sxs-lookup"><span data-stu-id="cb6f9-107">In this tutorial, you write the model classes first, and EF Core creates the database.</span></span> <span data-ttu-id="cb6f9-108">Hay un enfoque alternativo, que no se trata aquí, que consiste en [generar clases del modelo a partir de una base de datos existente](/ef/core/get-started/aspnetcore/existing-db).</span><span class="sxs-lookup"><span data-stu-id="cb6f9-108">An alternate approach not covered here is to [generate model classes from an existing database](/ef/core/get-started/aspnetcore/existing-db).</span></span>

<span data-ttu-id="cb6f9-109">[Vea o descargue](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie) un ejemplo.</span><span class="sxs-lookup"><span data-stu-id="cb6f9-109">[View or download](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie) sample.</span></span>
