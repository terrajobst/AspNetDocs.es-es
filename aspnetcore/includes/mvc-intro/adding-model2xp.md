---
ms.openlocfilehash: 0084d8c6bc5d16eaa1c3fa36d8aabb2aa31e1283
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/01/2019
ms.locfileid: "57029472"
---
<a name="cli"></a>
## <a name="perform-initial-migration"></a>Realizar la migración inicial

Desde la línea de comandos, ejecute los comandos CLI de .NET Core:

```console
dotnet ef migrations add InitialCreate
dotnet ef database update
```

El comando `dotnet ef migrations add InitialCreate` genera el código para crear el esquema de base de datos inicial. El esquema se basa en el modelo especificado en `DbContext` (en el archivo *Models/MvcMovieContext.cs*). El argumento `Initial` se usa para asignar nombre a las migraciones. Puede usar cualquier nombre, pero se suele elegir uno que describa la migración. Vea [Introduction to migrations](xref:data/ef-mvc/migrations#introduction-to-migrations) (Introducción a las migraciones) para obtener más información.

El comando `dotnet ef database update` ejecuta el método `Up` en el archivo *Migrations/\<time-stamp>_InitialCreate.cs*, con lo que se crea la base de datos.
