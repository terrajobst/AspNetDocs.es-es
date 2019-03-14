---
ms.openlocfilehash: 476580d4bc2435f73ef37c5344ab7fea4b67b9b8
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/01/2019
ms.locfileid: "57032502"
---
Para confiar en el certificado de desarrollo de HTTPS, ejecute el comando siguiente:

```console
dotnet dev-certs https --trust
```

El comando anterior muestra el siguiente cuadro de diálogo:

![Cuadro de diálogo de advertencia de seguridad](~/getting-started/_static/cert.png)

Si acepta confiar en el certificado de desarrollo, seleccione **Sí**.

Para obtener más información, vea [Confiar en el certificado de desarrollo de ASP.NET Core HTTPS ](xref:security/enforcing-ssl#trust-the-aspnet-core-https-development-certificate-on-windows-and-macos).