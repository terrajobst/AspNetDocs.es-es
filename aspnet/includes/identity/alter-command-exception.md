---
ms.openlocfilehash: b90e7963c5d9e5ef09fb519b72672c63bdffabee
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/01/2019
ms.locfileid: "57188722"
---
> <span data-ttu-id="dd4f8-101">Algunos comandos no se admiten si la aplicación usa SQLite como su almacén de datos de identidad.</span><span class="sxs-lookup"><span data-stu-id="dd4f8-101">Some commands aren't supported if the app uses SQLite as its Identity data store.</span></span> <span data-ttu-id="dd4f8-102">Debido a limitaciones en el motor de base de datos, `Alter` comandos producen la excepción siguiente:</span><span class="sxs-lookup"><span data-stu-id="dd4f8-102">Due to limitations in the database engine, `Alter` commands throw the following exception:</span></span>
>
> <span data-ttu-id="dd4f8-103">"System.NotSupportedException: SQLite no admite esta operación de migración."</span><span class="sxs-lookup"><span data-stu-id="dd4f8-103">"System.NotSupportedException: SQLite does not support this migration operation."</span></span> 
>
> <span data-ttu-id="dd4f8-104">Como solución alternativa, ejecute migraciones de Code First en la base de datos para cambiar las tablas.</span><span class="sxs-lookup"><span data-stu-id="dd4f8-104">As a work around, run Code First migrations on the database to change the tables.</span></span>
