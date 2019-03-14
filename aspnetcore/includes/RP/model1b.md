---
ms.openlocfilehash: 025a244812a50709bfd48de9f47a234a547c53e2
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/01/2019
ms.locfileid: "57047422"
---
<!-- THIS INCLUDE USED BY MVC AND RP --> Agregue las propiedades siguientes a la clase `Movie`:

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Models/Movie.cs?name=snippet1)]

la clase `Movie` contiene:

* La base de datos requiere el campo `ID` para la clave principal.
* `[DataType(DataType.Date)]`:  El atributo [DataType](/dotnet/api/microsoft.aspnetcore.mvc.dataannotations.internal.datatypeattributeadapter) especifica el tipo de datos (Date). Con este atributo:

  * El usuario no tiene que especificar información horaria en el campo de fecha.
  * Solo se muestra la fecha, no información horaria.

Los elementos [DataAnnotations](/dotnet/api/system.componentmodel.dataannotations) se tratan en un tutorial posterior.