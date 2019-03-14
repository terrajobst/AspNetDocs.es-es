---
ms.openlocfilehash: 0084d8c6bc5d16eaa1c3fa36d8aabb2aa31e1283
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/01/2019
ms.locfileid: "57029472"
---
<a name="cli"></a>
## <a name="perform-initial-migration"></a><span data-ttu-id="b524f-101">Realizar la migración inicial</span><span class="sxs-lookup"><span data-stu-id="b524f-101">Perform initial migration</span></span>

<span data-ttu-id="b524f-102">Desde la línea de comandos, ejecute los comandos CLI de .NET Core:</span><span class="sxs-lookup"><span data-stu-id="b524f-102">From the command line, run the following .NET Core CLI commands:</span></span>

```console
dotnet ef migrations add InitialCreate
dotnet ef database update
```

<span data-ttu-id="b524f-103">El comando `dotnet ef migrations add InitialCreate` genera el código para crear el esquema de base de datos inicial.</span><span class="sxs-lookup"><span data-stu-id="b524f-103">The `dotnet ef migrations add InitialCreate` command generates code to create the initial database schema.</span></span> <span data-ttu-id="b524f-104">El esquema se basa en el modelo especificado en `DbContext` (en el archivo *Models/MvcMovieContext.cs*).</span><span class="sxs-lookup"><span data-stu-id="b524f-104">The schema is based on the model specified in the `DbContext` (In the *Models/MvcMovieContext.cs* file).</span></span> <span data-ttu-id="b524f-105">El argumento `Initial` se usa para asignar nombre a las migraciones.</span><span class="sxs-lookup"><span data-stu-id="b524f-105">The `Initial` argument is used to name the migrations.</span></span> <span data-ttu-id="b524f-106">Puede usar cualquier nombre, pero se suele elegir uno que describa la migración.</span><span class="sxs-lookup"><span data-stu-id="b524f-106">You can use any name, but by convention you choose a name that describes the migration.</span></span> <span data-ttu-id="b524f-107">Vea [Introduction to migrations](xref:data/ef-mvc/migrations#introduction-to-migrations) (Introducción a las migraciones) para obtener más información.</span><span class="sxs-lookup"><span data-stu-id="b524f-107">See [Introduction to migrations](xref:data/ef-mvc/migrations#introduction-to-migrations) for more information.</span></span>

<span data-ttu-id="b524f-108">El comando `dotnet ef database update` ejecuta el método `Up` en el archivo *Migrations/\<time-stamp>_InitialCreate.cs*, con lo que se crea la base de datos.</span><span class="sxs-lookup"><span data-stu-id="b524f-108">The `dotnet ef database update` command runs the `Up` method in the *Migrations/\<time-stamp>_InitialCreate.cs* file, which creates the database.</span></span>
