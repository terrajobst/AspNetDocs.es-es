---
ms.openlocfilehash: 96cd8c78178b02ad3eb4a471540950b4581af583
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/01/2019
ms.locfileid: "57033382"
---
# <a name="gdpr-sample"></a>Ejemplo de RGPD

* En *appsettings.json*, establezca `CheckNotConsentNeeded` a `false` a requieren el consentimiento; en caso contrario, se establece en true o se omite. Probar la aplicación con `CheckNotConsentNeeded` establecido en `false` y establezca en `true`.
* Creación de cookies esenciales y no esenciales con cada variación de `CheckConsentNeeded` y consentimiento concedido.
* Registrar un usuario.
* Eliminar las cookies.
