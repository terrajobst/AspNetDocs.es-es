---
ms.openlocfilehash: e40d28e9a7ca12efe45988fabef23dece893d428
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/01/2019
ms.locfileid: "57049112"
---
# <a name="how-to-buildrun-secure-user-data-sample"></a>Procedimiento para generar y ejecutar el ejemplo de datos de usuario segura

* Establezca la contraseña con la herramienta Secret Manager:

  `dotnet user-secrets set SeedUserPW <pw>`

* Actualización de la base de datos:

    `dotnet ef database update`

* Habilitar HTTPS en el proyecto
