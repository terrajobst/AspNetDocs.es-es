---
ms.openlocfilehash: 4d6b6309e6e15c364542b5d3ae296d53c6ea4358
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/01/2019
ms.locfileid: "57037402"
---
Requiere el código de base de datos de identidad generado [migraciones de Entity Framework Core](/ef/core/managing-schemas/migrations/). Cree una migración y actualización de la base de datos. Por ejemplo, ejecute los siguientes comandos:

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

En Visual Studio **Package Manager Console**:

```PMC
Add-Migration CreateIdentitySchema
Update-Database
```

# <a name="net-core-clitabnetcore-cli"></a>[CLI de .NET Core](#tab/netcore-cli)

```cli
dotnet ef migrations add CreateIdentitySchema
dotnet ef database update
```

------

El parámetro de nombre "CreateIdentitySchema" para el `Add-Migration` comando es arbitrario. `"CreateIdentitySchema"` Describe la migración.
