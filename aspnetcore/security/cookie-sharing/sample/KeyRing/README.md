---
ms.openlocfilehash: 17ae8088e2e570628883ea5f8d71e093e6ed7cc8
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/01/2019
ms.locfileid: "57043032"
---
# <a name="data-protection-key-folder"></a>Carpeta de clave de protección de datos

Este archivo es un marcador de posición para crear una carpeta compartida para las claves de protección de datos.

En una implementación de producción, coloque las claves fuera de la raíz de desarrollo y nunca en el repositorio los archivos en este directorio en el control de código fuente. Proteger las claves de protección de datos en los archivos con DPAPI o un X509Certificate.

Consulte [protección de datos en ASP.NET Core: API de consumidor, configuración, API de extensibilidad e implementación](https://docs.microsoft.com/aspnet/core/security/data-protection/) para obtener más información.
