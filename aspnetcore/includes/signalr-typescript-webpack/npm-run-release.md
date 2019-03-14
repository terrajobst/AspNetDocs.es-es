---
ms.openlocfilehash: 939231583679d469d01724cc1a405bd45e46d2c5
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/01/2019
ms.locfileid: "57175900"
---
```console
npm run release
```

<span data-ttu-id="ff29f-101">Este comando da como resultado la entrega de los activos del lado cliente cuando se ejecuta la aplicación.</span><span class="sxs-lookup"><span data-stu-id="ff29f-101">This command yields the client-side assets to be served when running the app.</span></span> <span data-ttu-id="ff29f-102">Los recursos se colocan en la carpeta *wwwroot*.</span><span class="sxs-lookup"><span data-stu-id="ff29f-102">The assets are placed in the *wwwroot* folder.</span></span>

<span data-ttu-id="ff29f-103">Webpack ha completado las tareas siguientes:</span><span class="sxs-lookup"><span data-stu-id="ff29f-103">Webpack completed the following tasks:</span></span>

* <span data-ttu-id="ff29f-104">Purgar el contenido del directorio *wwwroot*.</span><span class="sxs-lookup"><span data-stu-id="ff29f-104">Purged the contents of the *wwwroot* directory.</span></span>
* <span data-ttu-id="ff29f-105">Convertir TypeScript en JavaScript, proceso conocido como *transpilación*.</span><span class="sxs-lookup"><span data-stu-id="ff29f-105">Converted the TypeScript to JavaScript&mdash;a process known as *transpilation*.</span></span>
* <span data-ttu-id="ff29f-106">Alterar el código JavaScript generado para reducir el tamaño del archivo, proceso conocido como *minificación*.</span><span class="sxs-lookup"><span data-stu-id="ff29f-106">Mangled the generated JavaScript to reduce file size&mdash;a process known as *minification*.</span></span>
* <span data-ttu-id="ff29f-107">Copiar los archivos JavaScript, CSS y HTML procesados desde *src* en el directorio *wwwroot*.</span><span class="sxs-lookup"><span data-stu-id="ff29f-107">Copied the processed JavaScript, CSS, and HTML files from *src* to the *wwwroot* directory.</span></span>
* <span data-ttu-id="ff29f-108">Insertar los elementos siguientes en el archivo *wwwroot/index.html*:</span><span class="sxs-lookup"><span data-stu-id="ff29f-108">Injected the following elements into the *wwwroot/index.html* file:</span></span>
    * <span data-ttu-id="ff29f-109">Etiqueta `<link>`, que hace referencia al archivo *wwwroot/main.\<hash\>.css*.</span><span class="sxs-lookup"><span data-stu-id="ff29f-109">A `<link>` tag, referencing the *wwwroot/main.\<hash\>.css* file.</span></span> <span data-ttu-id="ff29f-110">Esta etiqueta se coloca inmediatamente antes de la etiqueta `</head>` de cierre.</span><span class="sxs-lookup"><span data-stu-id="ff29f-110">This tag is placed immediately before the closing `</head>` tag.</span></span>
    * <span data-ttu-id="ff29f-111">Etiqueta `<script>`, que hace referencia al archivo *wwwroot/main.\<hash\>.js* minificado.</span><span class="sxs-lookup"><span data-stu-id="ff29f-111">A `<script>` tag, referencing the minified *wwwroot/main.\<hash\>.js* file.</span></span> <span data-ttu-id="ff29f-112">Esta etiqueta se coloca inmediatamente antes de la etiqueta `</body>` de cierre.</span><span class="sxs-lookup"><span data-stu-id="ff29f-112">This tag is placed immediately before the closing `</body>` tag.</span></span>
