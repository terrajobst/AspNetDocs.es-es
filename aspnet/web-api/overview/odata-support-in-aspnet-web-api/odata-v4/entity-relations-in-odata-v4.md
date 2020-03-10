---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/entity-relations-in-odata-v4
title: Relaciones de entidad en OData V4 con ASP.NET Web API 2,2 | Microsoft Docs
author: MikeWasson
description: 'La mayoría de los conjuntos de datos definen las relaciones entre las entidades: los clientes tienen pedidos; los libros tienen autores; los productos tienen proveedores. Con OData, los clientes pueden navegar por...'
ms.author: riande
ms.date: 06/26/2014
ms.assetid: 72657550-ec09-4779-9bfc-2fb15ecd51c7
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/entity-relations-in-odata-v4
msc.type: authoredcontent
ms.openlocfilehash: fbafb2b2346689271905db5790cdddeeb809b070
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78484465"
---
# <a name="entity-relations-in-odata-v4-using-aspnet-web-api-22"></a>Relaciones de entidad en OData V4 con ASP.NET Web API 2,2

por [Mike Wasson](https://github.com/MikeWasson)

> La mayoría de los conjuntos de datos definen las relaciones entre las entidades: los clientes tienen pedidos; los libros tienen autores; los productos tienen proveedores. Con OData, los clientes pueden navegar por las relaciones de entidad. Dado un producto, puede encontrar el proveedor. También puede crear o quitar relaciones. Por ejemplo, puede establecer el proveedor de un producto.
>
> En este tutorial se muestra cómo admitir estas operaciones en OData V4 mediante ASP.NET Web API. El tutorial se basa en el tutorial [creación de un punto de conexión de oData V4 mediante ASP.net web API 2](create-an-odata-v4-endpoint.md).
>
> ## <a name="software-versions-used-in-the-tutorial"></a>Versiones de software usadas en el tutorial
>
> - API Web 2,1
> - OData v4
> - Visual Studio 2013 (Descargue Visual Studio 2017 [aquí](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017))
> - Entity Framework 6
> - .NET 4.5
>
> ## <a name="tutorial-versions"></a>Versiones del tutorial
>
> Para la versión 3 de OData, consulte [compatibilidad con las relaciones de entidad en OData V3](https://asp.net/web-api/overview/odata-support-in-aspnet-web-api/odata-v3/working-with-entity-relations).

## <a name="add-a-supplier-entity"></a>Agregar una entidad de proveedor

> [!NOTE]
> El tutorial se basa en el tutorial [creación de un punto de conexión de oData V4 mediante ASP.net web API 2](create-an-odata-v4-endpoint.md).

En primer lugar, necesitamos una entidad relacionada. Agregue una clase denominada `Supplier` en la carpeta models.

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample1.cs)]

Agregue una propiedad de navegación a la clase `Product`:

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample2.cs?highlight=13-15)]

Agregue un nuevo **DbSet** a la clase `ProductsContext`, de modo que Entity Framework incluirá la tabla de proveedores en la base de datos.

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample3.cs?highlight=10)]

En WebApiConfig.cs, agregue un conjunto de entidades&quot; de &quot;Suppliers al Entity Data Model:

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample4.cs?highlight=6)]

## <a name="add-a-suppliers-controller"></a>Agregar un controlador de proveedores

Agregue una clase `SuppliersController` a la carpeta Controllers.

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample5.cs)]

No se muestra cómo agregar operaciones CRUD para este controlador. Los pasos son los mismos que para el controlador de productos (consulte [creación de un punto de conexión de oData V4](create-an-odata-v4-endpoint.md)).

## <a name="getting-related-entities"></a>Obtener entidades relacionadas

Para obtener el proveedor de un producto, el cliente envía una solicitud GET:

[!code-console[Main](entity-relations-in-odata-v4/samples/sample6.cmd)]

Para admitir esta solicitud, agregue el método siguiente a la clase `ProductsController`:

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample7.cs)]

Este método usa una Convención de nomenclatura predeterminada

- Nombre del método: GetX, donde X es la propiedad de navegación.
- Nombre del parámetro: *clave*

Si sigue esta Convención de nomenclatura, la API Web asigna automáticamente la solicitud HTTP al método de controlador.

Solicitud HTTP de ejemplo:

[!code-console[Main](entity-relations-in-odata-v4/samples/sample8.cmd)]

Respuesta HTTP de ejemplo:

[!code-console[Main](entity-relations-in-odata-v4/samples/sample9.cmd)]

### <a name="getting-a-related-collection"></a>Obtener una colección relacionada

En el ejemplo anterior, un producto tiene un proveedor. Una propiedad de navegación también puede devolver una colección. El código siguiente obtiene los productos de un proveedor:

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample10.cs)]

En este caso, el método devuelve un **IQueryable** en lugar de un **SingleResult&lt;t&gt;**

Solicitud HTTP de ejemplo:

[!code-console[Main](entity-relations-in-odata-v4/samples/sample11.cmd)]

Respuesta HTTP de ejemplo:

[!code-console[Main](entity-relations-in-odata-v4/samples/sample12.cmd)]

## <a name="creating-a-relationship-between-entities"></a>Crear una relación entre entidades

OData admite la creación o eliminación de relaciones entre dos entidades existentes. En la terminología de OData V4, la relación es una referencia &quot;&quot;. (En OData V3, la relación se llamaba un *vínculo*. Las diferencias de Protocolo no importan para este tutorial).

Una referencia tiene su propio URI, con el formato `/Entity/NavigationProperty/$ref`. Por ejemplo, este es el URI para tratar la referencia entre un producto y su proveedor:

[!code-console[Main](entity-relations-in-odata-v4/samples/sample13.cmd)]

Para agregar una relación, el cliente envía una solicitud POST o PUT a esta dirección.

- PUT si la propiedad de navegación es una sola entidad, como `Product.Supplier`.
- POST si la propiedad de navegación es una colección, como `Supplier.Products`.

El cuerpo de la solicitud contiene el URI de la otra entidad de la relación. A continuación se muestra una solicitud de ejemplo:

[!code-console[Main](entity-relations-in-odata-v4/samples/sample14.cmd)]

En este ejemplo, el cliente envía una solicitud PUT a `/Products(6)/Supplier/$ref`, que es el URI $ref para la `Supplier` del producto con el identificador 6. Si la solicitud se realiza correctamente, el servidor envía una respuesta 204 (sin contenido):

[!code-console[Main](entity-relations-in-odata-v4/samples/sample15.cmd)]

Este es el método de controlador para agregar una relación a un `Product`:

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample16.cs)]

El parámetro *navigationProperty* especifica la relación que se va a establecer. (Si hay más de una propiedad de navegación en la entidad, puede agregar más instrucciones `case`).

El parámetro de *vínculo* contiene el URI del proveedor. Web API analiza automáticamente el cuerpo de la solicitud para obtener el valor de este parámetro.

Para buscar el proveedor, necesitamos el identificador (o clave), que forma parte del parámetro de *vínculo* . Para ello, utilice el siguiente método auxiliar:

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample17.cs)]

Básicamente, este método usa la biblioteca de OData para dividir la ruta de acceso del URI en segmentos, buscar el segmento que contiene la clave y convertir la clave en el tipo correcto.

## <a name="deleting-a-relationship-between-entities"></a>Eliminar una relación entre entidades

Para eliminar una relación, el cliente envía una solicitud HTTP DELETE al URI $ref:

[!code-console[Main](entity-relations-in-odata-v4/samples/sample18.cmd)]

Este es el método de controlador para eliminar la relación entre un producto y un proveedor:

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample19.cs)]

En este caso, `Product.Supplier` es el &quot;1&quot; final de una relación de uno a varios, por lo que puede quitar la relación simplemente estableciendo `Product.Supplier` en `null`.

En el &quot;muchos&quot; extremo de una relación, el cliente debe especificar la entidad relacionada que se va a quitar. Para ello, el cliente envía el URI de la entidad relacionada en la cadena de consulta de la solicitud. Por ejemplo, para quitar "producto 1" de "proveedor 1":

[!code-console[Main](entity-relations-in-odata-v4/samples/sample20.cmd?highlight=1)]

Para admitir esto en Web API, es necesario incluir un parámetro adicional en el método `DeleteRef`. Este es el método de controlador para quitar un producto de la relación de `Supplier.Products`.

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample21.cs)]

El parámetro *key* es la clave del proveedor y el parámetro *relatedKey* es la clave del producto que se va a quitar de la relación `Products`. Tenga en cuenta que la API Web obtiene automáticamente la clave de la cadena de consulta.
