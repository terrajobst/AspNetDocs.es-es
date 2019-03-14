---
ms.openlocfilehash: d875717d480fe234ac5a328493fcec6a268e37a8
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/01/2019
ms.locfileid: "57039522"
---
<a name="scaffold"></a>
### <a name="scaffold-the-movie-model"></a>Aplicar scaffolding al modelo de película

* Ejecute lo siguiente desde la línea de comandos (en el directorio del proyecto que contiene los archivos *Program.cs*, *Startup.cs* y *.csproj*):

  ```console
  dotnet aspnet-codegenerator razorpage -m Movie -dc MovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  ```

Si se produce un error:
  ```
No executable found matching command "dotnet-aspnet-codegenerator"
  ```

Abra un shell de comandos en el directorio del proyecto (el directorio que contiene los archivos *Program.cs*, *Startup.cs* y *.csproj*).
