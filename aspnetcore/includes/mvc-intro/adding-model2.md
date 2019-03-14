---
ms.openlocfilehash: f323482d6f8bfaebf7bf6673d5fb91608430760a
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/01/2019
ms.locfileid: "57044082"
---
## <a name="add-initial-migration-and-update-the-database"></a>Agregar una migración inicial y actualizar la base de datos

* Abra un símbolo del sistema y desplácese al directorio del proyecto. (El directorio que contiene el *Startup.cs* archivo).

* Ejecute los siguientes comandos en el símbolo del sistema:

  ```console
  dotnet restore
  dotnet ef migrations add Initial
  dotnet ef database update
  ```
  
  [.NET core](/dotnet/core/tools/index) es una implementación multiplataforma de. NET. Esto es lo que hacen estos comandos:

  * [dotnet restore](/dotnet/core/tools/dotnet-restore): Descarga los paquetes de NuGet especificados en el *.csproj* archivo.
  * `dotnet ef migrations add Initial` Ejecuta el comando de migraciones de Entity Framework .NET Core CLI y crea la migración inicial. El parámetro después de "Agregar" es un nombre que se asigna a la migración. Aquí le da un nombre la migración "Inicial" porque es la migración de base de datos inicial. Esta operación crea el */migraciones de datos/\<de fecha y hora > _Initial.cs* archivo que contiene los comandos de migración para agregar la *película* tabla a la base de datos.
  * `dotnet ef database update`  Actualiza la base de datos con la migración que acabamos de crear.

Aprenderá acerca de la cadena de conexión y la base de datos en el siguiente tutorial. Obtendrá información sobre los cambios del modelo de datos en el [agregar un campo](xref:tutorials/first-mvc-app/new-field) tutorial.
