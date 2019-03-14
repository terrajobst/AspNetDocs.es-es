---
ms.openlocfilehash: 2481b10abae9aefce2d5173d8071e4c2236db928
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/01/2019
ms.locfileid: "57048222"
---

> [!NOTE]
> Muchas operaciones de cambio de esquema no se admiten con el proveedor de SQLite de EF Core. Por ejemplo, se permite agregar una columna, pero no eliminarla. Si se crea una migración para quitar una columna, el `ef migrations add` comando se ejecuta correctamente pero la `ef database update` comando produce un error. Algunas de estas limitaciones se pueden resolver escribiendo manualmente el código de las migraciones para llevar a cabo una recompilación de la tabla. Implica una recompilación de la tabla:

>* Cambiar el nombre de la tabla existente.
>* Crear una nueva tabla.
>* Copiar datos desde la tabla antigua a la nueva tabla.
>* Eliminación de la tabla anterior.

Para obtener más información, vea los siguientes recursos:
> * [Limitaciones del proveedor de base de datos SQLite para EF Core](/ef/core/providers/sqlite/limitations)
> * [Personalización del código de migración](/ef/core/managing-schemas/migrations/#customize-migration-code)
> * [Propagación de los datos](/ef/core/modeling/data-seeding)