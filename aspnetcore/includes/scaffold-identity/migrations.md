---
ms.openlocfilehash: 4d6b6309e6e15c364542b5d3ae296d53c6ea4358
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/01/2019
ms.locfileid: "57037402"
---
<span data-ttu-id="0d0b1-101">Requiere el código de base de datos de identidad generado [migraciones de Entity Framework Core](/ef/core/managing-schemas/migrations/).</span><span class="sxs-lookup"><span data-stu-id="0d0b1-101">The generated Identity database code requires [Entity Framework Core Migrations](/ef/core/managing-schemas/migrations/).</span></span> <span data-ttu-id="0d0b1-102">Cree una migración y actualización de la base de datos.</span><span class="sxs-lookup"><span data-stu-id="0d0b1-102">Create a migration and update the database.</span></span> <span data-ttu-id="0d0b1-103">Por ejemplo, ejecute los siguientes comandos:</span><span class="sxs-lookup"><span data-stu-id="0d0b1-103">For example, run the following commands:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="0d0b1-104">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="0d0b1-104">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="0d0b1-105">En Visual Studio **Package Manager Console**:</span><span class="sxs-lookup"><span data-stu-id="0d0b1-105">In the Visual Studio **Package Manager Console**:</span></span>

```PMC
Add-Migration CreateIdentitySchema
Update-Database
```

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="0d0b1-106">CLI de .NET Core</span><span class="sxs-lookup"><span data-stu-id="0d0b1-106">.NET Core CLI</span></span>](#tab/netcore-cli)

```cli
dotnet ef migrations add CreateIdentitySchema
dotnet ef database update
```

------

<span data-ttu-id="0d0b1-107">El parámetro de nombre "CreateIdentitySchema" para el `Add-Migration` comando es arbitrario.</span><span class="sxs-lookup"><span data-stu-id="0d0b1-107">The "CreateIdentitySchema" name parameter for the `Add-Migration` command is arbitrary.</span></span> <span data-ttu-id="0d0b1-108">`"CreateIdentitySchema"` Describe la migración.</span><span class="sxs-lookup"><span data-stu-id="0d0b1-108">`"CreateIdentitySchema"` describes the migration.</span></span>
