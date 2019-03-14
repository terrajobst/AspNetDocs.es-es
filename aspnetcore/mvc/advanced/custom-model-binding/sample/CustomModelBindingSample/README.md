---
ms.openlocfilehash: 71a40fcab8f1d1005cbe9327d01a42484765a421
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/01/2019
ms.locfileid: "57059642"
---
# <a name="custom-model-binding-demo"></a><span data-ttu-id="7bb54-101">Demostración de enlace de modelos personalizado</span><span class="sxs-lookup"><span data-stu-id="7bb54-101">Custom Model Binding Demo</span></span>

<span data-ttu-id="7bb54-102">Pruebe `ByteArrayModelBinder`; para ello, ejecute la aplicación y publique una cadena codificada en base64 en el punto de conexión `ImageController` (`/api/image/`).</span><span class="sxs-lookup"><span data-stu-id="7bb54-102">Test `ByteArrayModelBinder` by running the app and POSTing a base64-encoded string to the `ImageController` endpoint (`/api/image/`).</span></span> <span data-ttu-id="7bb54-103">Especifique las propiedades de archivo y nombre de archivo en el cuerpo de la solicitud como datos de formulario (con [Postman](https://www.getpostman.com/) o con otra herramienta similar).</span><span class="sxs-lookup"><span data-stu-id="7bb54-103">Specify the file and filename properties in the request body as form-data (using [Postman](https://www.getpostman.com/) or a similar tool).</span></span> <span data-ttu-id="7bb54-104">Puede usar [esta cadena de ejemplo](Base64String.txt).</span><span class="sxs-lookup"><span data-stu-id="7bb54-104">You can use [this sample string](Base64String.txt).</span></span> <span data-ttu-id="7bb54-105">El resultado se guardará en la carpeta *wwwroot/images/upload* con el nombre de archivo especificado.</span><span class="sxs-lookup"><span data-stu-id="7bb54-105">The result is saved in the *wwwroot/images/upload* folder with the filename specified.</span></span>

<span data-ttu-id="7bb54-106">Para probar el ejemplo de enlace personalizado, hágalo con los siguientes puntos de conexión:</span><span class="sxs-lookup"><span data-stu-id="7bb54-106">To test the custom binding example, try the following endpoints:</span></span>

* <span data-ttu-id="7bb54-107">/api/authors/1</span><span class="sxs-lookup"><span data-stu-id="7bb54-107">/api/authors/1</span></span>
* <span data-ttu-id="7bb54-108">/api/authors/2 (NOT FOUND)</span><span class="sxs-lookup"><span data-stu-id="7bb54-108">/api/authors/2 (NOT FOUND)</span></span>
* <span data-ttu-id="7bb54-109">/api/boundauthors/1</span><span class="sxs-lookup"><span data-stu-id="7bb54-109">/api/boundauthors/1</span></span>
* <span data-ttu-id="7bb54-110">/api/boundauthors/2 (NOT FOUND)</span><span class="sxs-lookup"><span data-stu-id="7bb54-110">/api/boundauthors/2 (NOT FOUND)</span></span>
* <span data-ttu-id="7bb54-111">/api/boundauthors/get/1</span><span class="sxs-lookup"><span data-stu-id="7bb54-111">/api/boundauthors/get/1</span></span>
* <span data-ttu-id="7bb54-112">/api/boundauthors/get/2 (NO CONTENT) &ndash; Esta acción no comprueba si hay valores null y devuelve un error *404 No encontrado*.</span><span class="sxs-lookup"><span data-stu-id="7bb54-112">/api/boundauthors/get/2 (NO CONTENT) &ndash; This action doesn't check for null and returns a *404 Not Found*.</span></span>
