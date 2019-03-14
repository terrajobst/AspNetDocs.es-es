---
ms.openlocfilehash: b90e7963c5d9e5ef09fb519b72672c63bdffabee
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/01/2019
ms.locfileid: "57188722"
---
> Algunos comandos no se admiten si la aplicación usa SQLite como su almacén de datos de identidad. Debido a limitaciones en el motor de base de datos, `Alter` comandos producen la excepción siguiente:
>
> "System.NotSupportedException: SQLite no admite esta operación de migración." 
>
> Como solución alternativa, ejecute migraciones de Code First en la base de datos para cambiar las tablas.
