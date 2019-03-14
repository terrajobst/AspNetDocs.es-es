---
ms.openlocfilehash: f323482d6f8bfaebf7bf6673d5fb91608430760a
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/01/2019
ms.locfileid: "57044082"
---
## <a name="add-initial-migration-and-update-the-database"></a><span data-ttu-id="c2269-101">Agregar una migración inicial y actualizar la base de datos</span><span class="sxs-lookup"><span data-stu-id="c2269-101">Add initial migration and update the database</span></span>

* <span data-ttu-id="c2269-102">Abra un símbolo del sistema y desplácese al directorio del proyecto.</span><span class="sxs-lookup"><span data-stu-id="c2269-102">Open a command prompt and navigate to the project directory.</span></span> <span data-ttu-id="c2269-103">(El directorio que contiene el *Startup.cs* archivo).</span><span class="sxs-lookup"><span data-stu-id="c2269-103">(The directory containing the *Startup.cs* file).</span></span>

* <span data-ttu-id="c2269-104">Ejecute los siguientes comandos en el símbolo del sistema:</span><span class="sxs-lookup"><span data-stu-id="c2269-104">Run the following commands in the command prompt:</span></span>

  ```console
  dotnet restore
  dotnet ef migrations add Initial
  dotnet ef database update
  ```
  
  <span data-ttu-id="c2269-105">[.NET core](/dotnet/core/tools/index) es una implementación multiplataforma de. NET.</span><span class="sxs-lookup"><span data-stu-id="c2269-105">[.NET Core](/dotnet/core/tools/index) is a cross-platform implementation of .NET.</span></span> <span data-ttu-id="c2269-106">Esto es lo que hacen estos comandos:</span><span class="sxs-lookup"><span data-stu-id="c2269-106">Here is what these commands do:</span></span>

  * <span data-ttu-id="c2269-107">[dotnet restore](/dotnet/core/tools/dotnet-restore): Descarga los paquetes de NuGet especificados en el *.csproj* archivo.</span><span class="sxs-lookup"><span data-stu-id="c2269-107">[dotnet restore](/dotnet/core/tools/dotnet-restore): Downloads the NuGet packages specified in the *.csproj* file.</span></span>
  * <span data-ttu-id="c2269-108">`dotnet ef migrations add Initial` Ejecuta el comando de migraciones de Entity Framework .NET Core CLI y crea la migración inicial.</span><span class="sxs-lookup"><span data-stu-id="c2269-108">`dotnet ef migrations add Initial` Runs the Entity Framework .NET Core CLI migrations command and creates the initial migration.</span></span> <span data-ttu-id="c2269-109">El parámetro después de "Agregar" es un nombre que se asigna a la migración.</span><span class="sxs-lookup"><span data-stu-id="c2269-109">The parameter after "add" is a name that you assign to the migration.</span></span> <span data-ttu-id="c2269-110">Aquí le da un nombre la migración "Inicial" porque es la migración de base de datos inicial.</span><span class="sxs-lookup"><span data-stu-id="c2269-110">Here you're naming the migration "Initial" because it's the initial database migration.</span></span> <span data-ttu-id="c2269-111">Esta operación crea el */migraciones de datos/\<de fecha y hora > _Initial.cs* archivo que contiene los comandos de migración para agregar la *película* tabla a la base de datos.</span><span class="sxs-lookup"><span data-stu-id="c2269-111">This operation creates the *Data/Migrations/\<date-time>_Initial.cs* file containing the migration commands to add the *Movie* table to the database.</span></span>
  * <span data-ttu-id="c2269-112">`dotnet ef database update`  Actualiza la base de datos con la migración que acabamos de crear.</span><span class="sxs-lookup"><span data-stu-id="c2269-112">`dotnet ef database update`  Updates the database with the migration we just created.</span></span>

<span data-ttu-id="c2269-113">Aprenderá acerca de la cadena de conexión y la base de datos en el siguiente tutorial.</span><span class="sxs-lookup"><span data-stu-id="c2269-113">You'll learn about the database and connection string in the next tutorial.</span></span> <span data-ttu-id="c2269-114">Obtendrá información sobre los cambios del modelo de datos en el [agregar un campo](xref:tutorials/first-mvc-app/new-field) tutorial.</span><span class="sxs-lookup"><span data-stu-id="c2269-114">You'll learn about data model changes in the [Add a field](xref:tutorials/first-mvc-app/new-field) tutorial.</span></span>
