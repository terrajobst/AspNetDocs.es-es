---
ms.openlocfilehash: 545448e3673b02abc7e685bd987f2cf5f71375b4
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/01/2019
ms.locfileid: "57051962"
---
> [!WARNING]
> <span data-ttu-id="8fdb8-101">Por motivos de seguridad, debe participar en el enlace de datos de solicitud `GET` con las propiedades del modelo de página.</span><span class="sxs-lookup"><span data-stu-id="8fdb8-101">For security reasons, you must opt in to binding `GET` request data to page model properties.</span></span> <span data-ttu-id="8fdb8-102">Compruebe las entradas de los usuarios antes de asignarlas a las propiedades.</span><span class="sxs-lookup"><span data-stu-id="8fdb8-102">Verify user input before mapping it to properties.</span></span> <span data-ttu-id="8fdb8-103">Si participa en el enlace de `GET`, le puede ser útil al trabajar con escenarios que dependan de cadenas de consultas o valores de rutas.</span><span class="sxs-lookup"><span data-stu-id="8fdb8-103">Opting in to `GET` binding is useful when addressing scenarios which rely on query string or route values.</span></span>
>
> <span data-ttu-id="8fdb8-104">Para enlazar una propiedad en solicitudes `GET`, establezca la propiedad `SupportsGet` del atributo [[BindProperty]](/dotnet/api/microsoft.aspnetcore.mvc.bindpropertyattribute) como `true`: `[BindProperty(SupportsGet = true)]`</span><span class="sxs-lookup"><span data-stu-id="8fdb8-104">To bind a property on `GET` requests, set the [[BindProperty]](/dotnet/api/microsoft.aspnetcore.mvc.bindpropertyattribute) attribute's `SupportsGet` property to `true`: `[BindProperty(SupportsGet = true)]`</span></span>
