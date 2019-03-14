---
ms.openlocfilehash: 025a244812a50709bfd48de9f47a234a547c53e2
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/01/2019
ms.locfileid: "57047422"
---
<span data-ttu-id="3e1af-101"><!-- THIS INCLUDE USED BY MVC AND RP --> Agregue las propiedades siguientes a la clase `Movie`:</span><span class="sxs-lookup"><span data-stu-id="3e1af-101"><!-- THIS INCLUDE USED BY MVC AND RP --> Add the following properties to the `Movie` class:</span></span>

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Models/Movie.cs?name=snippet1)]

<span data-ttu-id="3e1af-102">la clase `Movie` contiene:</span><span class="sxs-lookup"><span data-stu-id="3e1af-102">The `Movie` class contains:</span></span>

* <span data-ttu-id="3e1af-103">La base de datos requiere el campo `ID` para la clave principal.</span><span class="sxs-lookup"><span data-stu-id="3e1af-103">The `ID` field is required by the database for the primary key.</span></span>
* <span data-ttu-id="3e1af-104">`[DataType(DataType.Date)]`:  El atributo [DataType](/dotnet/api/microsoft.aspnetcore.mvc.dataannotations.internal.datatypeattributeadapter) especifica el tipo de datos (Date).</span><span class="sxs-lookup"><span data-stu-id="3e1af-104">`[DataType(DataType.Date)]`:  The [DataType](/dotnet/api/microsoft.aspnetcore.mvc.dataannotations.internal.datatypeattributeadapter) attribute specifies the type of the data (Date).</span></span> <span data-ttu-id="3e1af-105">Con este atributo:</span><span class="sxs-lookup"><span data-stu-id="3e1af-105">With this attribute:</span></span>

  * <span data-ttu-id="3e1af-106">El usuario no tiene que especificar información horaria en el campo de fecha.</span><span class="sxs-lookup"><span data-stu-id="3e1af-106">The user is not required to enter time information in the date field.</span></span>
  * <span data-ttu-id="3e1af-107">Solo se muestra la fecha, no información horaria.</span><span class="sxs-lookup"><span data-stu-id="3e1af-107">Only the date is displayed, not time information.</span></span>

<span data-ttu-id="3e1af-108">Los elementos [DataAnnotations](/dotnet/api/system.componentmodel.dataannotations) se tratan en un tutorial posterior.</span><span class="sxs-lookup"><span data-stu-id="3e1af-108">[DataAnnotations](/dotnet/api/system.componentmodel.dataannotations) are covered in a later tutorial.</span></span>