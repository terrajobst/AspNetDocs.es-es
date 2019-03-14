---
ms.openlocfilehash: 2481b10abae9aefce2d5173d8071e4c2236db928
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/01/2019
ms.locfileid: "57048222"
---

> [!NOTE]
> <span data-ttu-id="9168a-101">Muchas operaciones de cambio de esquema no se admiten con el proveedor de SQLite de EF Core.</span><span class="sxs-lookup"><span data-stu-id="9168a-101">Many schema change operations are not supported by the EF Core SQLite provider.</span></span> <span data-ttu-id="9168a-102">Por ejemplo, se permite agregar una columna, pero no eliminarla.</span><span class="sxs-lookup"><span data-stu-id="9168a-102">For example, adding a column is supported, but removing a column is not supported.</span></span> <span data-ttu-id="9168a-103">Si se crea una migración para quitar una columna, el `ef migrations add` comando se ejecuta correctamente pero la `ef database update` comando produce un error.</span><span class="sxs-lookup"><span data-stu-id="9168a-103">If a migration is created to remove a column, the `ef migrations add` command succeeds but the `ef database update` command fails.</span></span> <span data-ttu-id="9168a-104">Algunas de estas limitaciones se pueden resolver escribiendo manualmente el código de las migraciones para llevar a cabo una recompilación de la tabla.</span><span class="sxs-lookup"><span data-stu-id="9168a-104">Some of these limitations can be overcome by manually writing migrations code to perform a table rebuild.</span></span> <span data-ttu-id="9168a-105">Implica una recompilación de la tabla:</span><span class="sxs-lookup"><span data-stu-id="9168a-105">A table rebuild involves:</span></span>

>* <span data-ttu-id="9168a-106">Cambiar el nombre de la tabla existente.</span><span class="sxs-lookup"><span data-stu-id="9168a-106">Renaming the existing table.</span></span>
>* <span data-ttu-id="9168a-107">Crear una nueva tabla.</span><span class="sxs-lookup"><span data-stu-id="9168a-107">Creating a new table.</span></span>
>* <span data-ttu-id="9168a-108">Copiar datos desde la tabla antigua a la nueva tabla.</span><span class="sxs-lookup"><span data-stu-id="9168a-108">Copying data from the old table to the new table.</span></span>
>* <span data-ttu-id="9168a-109">Eliminación de la tabla anterior.</span><span class="sxs-lookup"><span data-stu-id="9168a-109">Dropping the old table.</span></span>

<span data-ttu-id="9168a-110">Para obtener más información, vea los siguientes recursos:</span><span class="sxs-lookup"><span data-stu-id="9168a-110">For more information, see the following resources:</span></span>
> * [<span data-ttu-id="9168a-111">Limitaciones del proveedor de base de datos SQLite para EF Core</span><span class="sxs-lookup"><span data-stu-id="9168a-111">SQLite EF Core Database Provider Limitations</span></span>](/ef/core/providers/sqlite/limitations)
> * [<span data-ttu-id="9168a-112">Personalización del código de migración</span><span class="sxs-lookup"><span data-stu-id="9168a-112">Customize migration code</span></span>](/ef/core/managing-schemas/migrations/#customize-migration-code)
> * [<span data-ttu-id="9168a-113">Propagación de los datos</span><span class="sxs-lookup"><span data-stu-id="9168a-113">Data seeding</span></span>](/ef/core/modeling/data-seeding)