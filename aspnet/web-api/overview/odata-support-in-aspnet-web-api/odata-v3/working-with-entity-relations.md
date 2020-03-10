---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v3/working-with-entity-relations
title: Compatibilidad con las relaciones de entidad en OData V3 con Web API 2 | Microsoft Docs
author: MikeWasson
description: 'La mayoría de los conjuntos de datos definen las relaciones entre las entidades: los clientes tienen pedidos; los libros tienen autores; los productos tienen proveedores. Con OData, los clientes pueden navegar por...'
ms.author: riande
ms.date: 02/26/2014
ms.assetid: 1e4c2eb4-b6cf-42ff-8a65-4d71ddca0394
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v3/working-with-entity-relations
msc.type: authoredcontent
ms.openlocfilehash: 726a7d51123805e05f6831ef9cd7eaa84b6c44bd
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78484507"
---
# <a name="supporting-entity-relations-in-odata-v3-with-web-api-2"></a>Compatibilidad de las relaciones de entidad en OData V3 con Web API 2

por [Mike Wasson](https://github.com/MikeWasson)

[Descargar proyecto completado](https://code.msdn.microsoft.com/ASPNET-Web-API-OData-cecdb524)

> La mayoría de los conjuntos de datos definen las relaciones entre las entidades: los clientes tienen pedidos; los libros tienen autores; los productos tienen proveedores. Con OData, los clientes pueden navegar por las relaciones de entidad. Dado un producto, puede encontrar el proveedor. También puede crear o quitar relaciones. Por ejemplo, puede establecer el proveedor de un producto.
> 
> En este tutorial se muestra cómo admitir estas operaciones en ASP.NET Web API. El tutorial se basa en el tutorial [creación de un punto de conexión de oData V3 con Web API 2](creating-an-odata-endpoint.md).
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versiones de software usadas en el tutorial
> 
> 
> - API Web 2
> - Versión 3 de OData
> - Entity Framework 6

## <a name="add-a-supplier-entity"></a>Agregar una entidad de proveedor

En primer lugar, es necesario agregar un nuevo tipo de entidad a la fuente de OData. Vamos a agregar una clase `Supplier`.

[!code-csharp[Main](working-with-entity-relations/samples/sample1.cs)]

Esta clase usa una cadena para la clave de entidad. En la práctica, esto podría ser menos común que usar una clave entera. Pero merece la pena ver cómo OData controla otros tipos de clave además de enteros.

A continuación, vamos a crear una relación agregando una propiedad `Supplier` a la clase `Product`:

[!code-csharp[Main](working-with-entity-relations/samples/sample2.cs)]

Agregue un nuevo **DbSet** a la clase `ProductServiceContext`, de modo que Entity Framework incluirá la tabla `Supplier` en la base de datos.

[!code-csharp[Main](working-with-entity-relations/samples/sample3.cs?highlight=9)]

En WebApiConfig.cs, agregue una entidad "Suppliers" al modelo EDM:

[!code-csharp[Main](working-with-entity-relations/samples/sample4.cs?highlight=4)]

## <a name="navigation-properties"></a>Propiedades de navegación

Para obtener el proveedor de un producto, el cliente envía una solicitud GET:

[!code-console[Main](working-with-entity-relations/samples/sample5.cmd)]

Aquí "proveedor" es una propiedad de navegación en el tipo de `Product`. En este caso, `Supplier` hace referencia a un único elemento, pero una propiedad de navegación también puede devolver una colección (relación de uno a varios o de varios a varios).

Para admitir esta solicitud, agregue el método siguiente a la clase `ProductsController`:

[!code-csharp[Main](working-with-entity-relations/samples/sample6.cs)]

El parámetro *key* es la clave del producto. El método devuelve la entidad&#8212;relacionada en este caso, una instancia de `Supplier`. El nombre del método y el nombre del parámetro son importantes. En general, si la propiedad de navegación se denomina "X", debe agregar un método denominado "GetX". El método debe tomar un parámetro denominado "*key*" que coincida con el tipo de datos de la clave del elemento primario.

También es importante incluir el atributo **[FromOdataUri]** en el parámetro *clave* . Este atributo indica a la API Web que use las reglas de sintaxis de OData cuando analiza la clave del URI de solicitud.

## <a name="creating-and-deleting-links"></a>Crear y eliminar vínculos

OData admite la creación o eliminación de relaciones entre dos entidades. En la terminología de OData, la relación es un "vínculo". Cada vínculo tiene un URI con el formato *entidad*/$links/*entidad*. Por ejemplo, el vínculo de producto a proveedor tiene el siguiente aspecto:

[!code-console[Main](working-with-entity-relations/samples/sample7.cmd)]

Para crear un vínculo nuevo, el cliente envía una solicitud POST al URI del vínculo. El cuerpo de la solicitud es el URI de la entidad de destino. Por ejemplo, supongamos que hay un proveedor con la clave "CTSO". Para crear un vínculo de "Product (1)" a "Supplier (' CTSO ')", el cliente envía una solicitud como la siguiente:

[!code-console[Main](working-with-entity-relations/samples/sample8.cmd)]

Para eliminar un vínculo, el cliente envía una solicitud DELETE al URI del vínculo.

**Crear vínculos**

Para permitir que un cliente cree vínculos de proveedor de productos, agregue el código siguiente a la clase `ProductsController`:

[!code-csharp[Main](working-with-entity-relations/samples/sample9.cs)]

Este método usa tres parámetros:

- *key*: la clave de la entidad primaria (el producto)
- *navigationProperty*: el nombre de la propiedad de navegación. En este ejemplo, la única propiedad de navegación válida es "proveedor".
- *Link*: el URI de oData de la entidad relacionada. Este valor se toma del cuerpo de la solicitud. Por ejemplo, el URI del vínculo podría ser "`http://localhost/odata/Suppliers('CTSO')`, lo que significa que el proveedor con ID. = ' CTSO '.

El método utiliza el vínculo para buscar el proveedor. Si se encuentra el proveedor coincidente, el método establece la propiedad `Product.Supplier` y guarda el resultado en la base de datos.

La parte más difícil es analizar el URI del vínculo. Básicamente, debe simular el resultado de enviar una solicitud GET a ese URI. El siguiente método auxiliar muestra cómo hacerlo. El método invoca el proceso de enrutamiento de la API Web y devuelve una instancia de **ODataPath** que representa la ruta de acceso de oData analizada. Para un URI de vínculo, uno de los segmentos debe ser la clave de entidad. (Si no es así, el cliente envió un URI incorrecto).

[!code-csharp[Main](working-with-entity-relations/samples/sample10.cs)]

**Eliminar vínculos**

Para eliminar un vínculo, agregue el código siguiente a la clase `ProductsController`:

[!code-csharp[Main](working-with-entity-relations/samples/sample11.cs)]

En este ejemplo, la propiedad de navegación es una entidad de `Supplier` única. Si la propiedad de navegación es una colección, el URI para eliminar un vínculo debe incluir una clave para la entidad relacionada. Por ejemplo:

[!code-console[Main](working-with-entity-relations/samples/sample12.cmd)]

Esta solicitud quita el pedido 1 del cliente 1. En este caso, el método DeleteLink tendrá la siguiente firma:

[!code-csharp[Main](working-with-entity-relations/samples/sample13.cs)]

El parámetro *relatedKey* proporciona la clave para la entidad relacionada. Por lo tanto, en el método de `DeleteLink`, busque la entidad principal mediante el parámetro de *clave* , busque la entidad relacionada mediante el parámetro *relatedKey* y, a continuación, quite la asociación. Dependiendo del modelo de datos, puede que tenga que implementar ambas versiones de `DeleteLink`. La API Web llamará a la versión correcta en función del URI de solicitud.
