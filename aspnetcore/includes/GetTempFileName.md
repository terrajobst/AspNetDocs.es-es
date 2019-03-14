---
ms.openlocfilehash: 2cd201e16cd491d0f468e8ef141b522f1694a257
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/01/2019
ms.locfileid: "57165863"
---
**Advertencia**: El siguiente código utiliza `GetTempFileName`, que produce una `IOException` si crea más de 65535 archivos sin eliminar los archivos temporales anteriores. Una aplicación real debe eliminar los archivos temporales o usar `GetTempPath` y `GetRandomFileName` para crear nombres de archivo temporales. El límite de 65 535 archivos es por servidor, por lo que otra aplicación en el servidor puede usar los 65 535 archivos. 
