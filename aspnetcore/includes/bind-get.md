---
ms.openlocfilehash: 545448e3673b02abc7e685bd987f2cf5f71375b4
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/01/2019
ms.locfileid: "57051962"
---
> [!WARNING]
> Por motivos de seguridad, debe participar en el enlace de datos de solicitud `GET` con las propiedades del modelo de página. Compruebe las entradas de los usuarios antes de asignarlas a las propiedades. Si participa en el enlace de `GET`, le puede ser útil al trabajar con escenarios que dependan de cadenas de consultas o valores de rutas.
>
> Para enlazar una propiedad en solicitudes `GET`, establezca la propiedad `SupportsGet` del atributo [[BindProperty]](/dotnet/api/microsoft.aspnetcore.mvc.bindpropertyattribute) como `true`: `[BindProperty(SupportsGet = true)]`
