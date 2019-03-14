---
ms.openlocfilehash: e40d28e9a7ca12efe45988fabef23dece893d428
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/01/2019
ms.locfileid: "57049112"
---
# <a name="how-to-buildrun-secure-user-data-sample"></a><span data-ttu-id="30ab2-101">Procedimiento para generar y ejecutar el ejemplo de datos de usuario segura</span><span class="sxs-lookup"><span data-stu-id="30ab2-101">How to build/run Secure user data sample</span></span>

* <span data-ttu-id="30ab2-102">Establezca la contraseña con la herramienta Secret Manager:</span><span class="sxs-lookup"><span data-stu-id="30ab2-102">Set password with the Secret Manager tool:</span></span>

  `dotnet user-secrets set SeedUserPW <pw>`

* <span data-ttu-id="30ab2-103">Actualización de la base de datos:</span><span class="sxs-lookup"><span data-stu-id="30ab2-103">Update the database:</span></span>

    `dotnet ef database update`

* <span data-ttu-id="30ab2-104">Habilitar HTTPS en el proyecto</span><span class="sxs-lookup"><span data-stu-id="30ab2-104">Enable HTTPS in the project</span></span>
