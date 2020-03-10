---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-security-guidance
title: Guía de seguridad para ASP.NET Web API 2 OData-ASP.NET 4. x
author: MikeWasson
description: Describe los problemas de seguridad que se deben tener en cuenta al exponer un conjunto de DataSet a través de OData para ASP.NET Web API 2 en ASP.NET 4. x.
ms.author: riande
ms.date: 02/06/2013
ms.custom: seoapril2019
ms.assetid: b91e6424-1544-4747-bd0b-d1f8418c9653
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-security-guidance
msc.type: authoredcontent
ms.openlocfilehash: 8194a368cb0629c30e32ec05bf4bed150d442ad8
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78448297"
---
# <a name="security-guidance-for-aspnet-web-api-2-odata"></a>Guía de seguridad para ASP.NET Web API 2 OData

por [Mike Wasson](https://github.com/MikeWasson)

En este tema se describen algunos de los problemas de seguridad que se deben tener en cuenta al exponer un conjunto de DataSet a través de OData para ASP.NET Web API 2 en ASP.NET 4. x.

## <a name="edm-security"></a>Seguridad de EDM

La semántica de la consulta se basa en el Entity Data Model (EDM), no en los tipos de modelo subyacentes. Puede excluir una propiedad del EDM y no será visible para la consulta. Por ejemplo, supongamos que el modelo incluye un tipo de empleado con una propiedad salary. Es posible que desee excluir esta propiedad del EDM para ocultarla de los clientes.

Hay dos maneras de excluir una propiedad del EDM. Puede establecer el atributo **[IgnoreDataMember]** en la propiedad en la clase de modelo:

[!code-csharp[Main](odata-security-guidance/samples/sample1.cs)]

También puede quitar la propiedad del EDM mediante programación:

[!code-csharp[Main](odata-security-guidance/samples/sample2.cs)]

## <a name="query-security"></a>Seguridad de las consultas

Un cliente malintencionado o Naive puede crear una consulta que tarda mucho tiempo en ejecutarse. En el peor de los casos, esto puede interrumpir el acceso a su servicio.

El atributo **[Queryable]** es un filtro de acción que analiza, valida y aplica la consulta. El filtro convierte las opciones de consulta en una expresión LINQ. Cuando el controlador OData devuelve un tipo **IQueryable** , el proveedor LINQ de **IQueryable** convierte la expresión LINQ en una consulta. Por lo tanto, el rendimiento depende del proveedor LINQ que se utiliza, y también de las características concretas del conjunto de datos o del esquema de la base de datos.

Para obtener más información sobre el uso de las opciones de consulta de OData en ASP.NET Web API, consulte [compatibilidad con las opciones de consulta de oData](supporting-odata-query-options.md).

Si sabe que todos los clientes son de confianza (por ejemplo, en un entorno empresarial) o si el conjunto de información es pequeño, el rendimiento de las consultas podría no ser un problema. De lo contrario, debe tener en cuenta las siguientes recomendaciones.

- Pruebe el servicio con varias consultas y realice el perfil de la base de BD.
- Habilitar la paginación controlada por el servidor, para evitar que se devuelva un conjunto de datos grande en una consulta. Para obtener más información, consulte [paginación controlada por el servidor](supporting-odata-query-options.md#server-paging). 

    [!code-csharp[Main](odata-security-guidance/samples/sample3.cs)]
- ¿Necesita $filter y $orderby? Algunas aplicaciones pueden permitir la paginación del cliente mediante $top y $skip, pero deshabilitar las demás opciones de consulta. 

    [!code-csharp[Main](odata-security-guidance/samples/sample4.cs)]
- Considere la posibilidad de restringir $orderby a las propiedades de un índice clúster. La ordenación de datos grandes sin un índice clúster es lenta. 

    [!code-csharp[Main](odata-security-guidance/samples/sample5.cs)]
- Número máximo de nodos: la propiedad **MaxNodeCount** en **[Queryable]** establece el número máximo de nodos permitido en el árbol de sintaxis $Filter. El valor predeterminado es 100, pero puede que desee establecer un valor inferior, ya que un gran número de nodos puede ser lento de la compilación. Esto es especialmente cierto si usa LINQ to Objects (es decir, consultas LINQ en una colección en memoria, sin usar un proveedor LINQ intermedio). 

    [!code-csharp[Main](odata-security-guidance/samples/sample6.cs)]
- Considere la posibilidad de deshabilitar las funciones any () y All (), ya que pueden ser lentas. 

    [!code-csharp[Main](odata-security-guidance/samples/sample7.cs)]
- Si alguna de las propiedades de cadena&#8212;contiene cadenas grandes, por ejemplo, una descripción del&#8212;producto o una entrada del blog considere la posibilidad de deshabilitar las funciones de cadena. 

    [!code-csharp[Main](odata-security-guidance/samples/sample8.cs)]
- Considere la posibilidad de no permitir el filtrado en las propiedades de navegación. El filtrado de las propiedades de navegación puede dar lugar a una combinación, que podría ser lenta, dependiendo del esquema de la base de datos. En el código siguiente se muestra un validador de consulta que impide el filtrado en las propiedades de navegación. Para obtener más información sobre los validadores de consultas, vea [validación de consultas](supporting-odata-query-options.md#query-validation). 

    [!code-csharp[Main](odata-security-guidance/samples/sample9.cs)]
- Considere la posibilidad de restringir las consultas de $filter escribiendo un validador personalizado para la base de datos. Por ejemplo, considere estas dos consultas: 

  - Todas las películas con actores cuyo apellido empieza por ' A '.
  - Todas las películas publicadas en 1994.

    A menos que los actores indexen las películas, es posible que la primera consulta requiera que el motor de base de BD examine toda la lista de películas. Mientras que la segunda consulta podría ser aceptable, suponiendo que las películas se indexan por año de la versión.

    En el código siguiente se muestra un validador que permite filtrar en las propiedades "ReleaseYear" y "title", pero no en otras propiedades.

    [!code-csharp[Main](odata-security-guidance/samples/sample10.cs)]
- En general, tenga en cuenta qué $filter funciones necesita. Si los clientes no necesitan la expresividad total de $filter, puede limitar las funciones permitidas.
