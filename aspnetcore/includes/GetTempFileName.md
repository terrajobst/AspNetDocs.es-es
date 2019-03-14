---
ms.openlocfilehash: 2cd201e16cd491d0f468e8ef141b522f1694a257
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/01/2019
ms.locfileid: "57165863"
---
<span data-ttu-id="eb72f-101">**Advertencia**: El siguiente código utiliza `GetTempFileName`, que produce una `IOException` si crea más de 65535 archivos sin eliminar los archivos temporales anteriores.</span><span class="sxs-lookup"><span data-stu-id="eb72f-101">**Warning**: The following code uses `GetTempFileName`, which throws an `IOException` if more than 65535 files are created without deleting previous temporary files.</span></span> <span data-ttu-id="eb72f-102">Una aplicación real debe eliminar los archivos temporales o usar `GetTempPath` y `GetRandomFileName` para crear nombres de archivo temporales.</span><span class="sxs-lookup"><span data-stu-id="eb72f-102">A real app should either delete temporary files or use `GetTempPath` and `GetRandomFileName` to create temporary file names.</span></span> <span data-ttu-id="eb72f-103">El límite de 65 535 archivos es por servidor, por lo que otra aplicación en el servidor puede usar los 65 535 archivos.</span><span class="sxs-lookup"><span data-stu-id="eb72f-103">The 65535 files limit is per server, so another app on the server can use up all 65535 files.</span></span> 
