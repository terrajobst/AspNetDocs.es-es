---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/odata-actions-and-functions
title: Acciones y funciones en OData V4 con ASP.NET Web API 2,2 | Microsoft Docs
author: MikeWasson
description: En OData, las acciones y las funciones son una manera de agregar comportamientos del lado servidor que no se definen fácilmente como operaciones CRUD en entidades. En este tutorial se muestra cómo...
ms.author: riande
ms.date: 06/27/2014
ms.assetid: 0e6fb03c-b16d-4bb0-ab0b-552bd2b6ece1
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/odata-actions-and-functions
msc.type: authoredcontent
ms.openlocfilehash: f5af94e93e5b7f2351d40febbf1a468d635c9db1
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78448063"
---
# <a name="actions-and-functions-in-odata-v4-using-aspnet-web-api-22"></a>Acciones y funciones en OData V4 mediante ASP.NET Web API 2,2

por [Mike Wasson](https://github.com/MikeWasson)

> En OData, las acciones y las funciones son una manera de agregar comportamientos del lado servidor que no se definen fácilmente como operaciones CRUD en entidades. En este tutorial se muestra cómo agregar acciones y funciones a un punto de conexión de OData V4 mediante Web API 2,2. El tutorial se basa en el tutorial [creación de un punto de conexión de oData V4 mediante ASP.net web API 2](create-an-odata-v4-endpoint.md)
>
> ## <a name="software-versions-used-in-the-tutorial"></a>Versiones de software usadas en el tutorial
>
> - API Web 2,2
> - OData v4
> - Visual Studio 2013 (Descargue Visual Studio 2017 [aquí](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017))
> - .NET 4.5
>
> ## <a name="tutorial-versions"></a>Versiones del tutorial
>
> Para la versión 3 de OData, consulte [acciones de oData en ASP.net web API 2](../odata-v3/odata-actions.md).

La diferencia entre *las acciones* y las *funciones* es que las acciones pueden tener efectos secundarios y las funciones no. Tanto las acciones como las funciones pueden devolver datos. Algunos usos de las acciones son:

- Transacciones complejas.
- Manipular varias entidades a la vez.
- Permitir actualizaciones solo en determinadas propiedades de una entidad.
- Enviar datos que no son una entidad.

Las funciones son útiles para devolver información que no se corresponde directamente con una entidad o colección.

Una acción (o función) puede tener como destino una sola entidad o una colección. En la terminología de OData, este es el *enlace*. También puede tener &quot;funciones o acciones de&quot; sin enlazar, a las que se llama como operaciones estáticas en el servicio.

## <a name="example-adding-an-action"></a>Ejemplo: agregar una acción

Vamos a definir una acción para clasificar un producto.

> [!NOTE]
> Este tutorial se basa en el tutorial [creación de un punto de conexión de oData V4 mediante ASP.net web API 2](create-an-odata-v4-endpoint.md)

En primer lugar, agregue un modelo de `ProductRating` para representar las clasificaciones.

[!code-csharp[Main](odata-actions-and-functions/samples/sample1.cs)]

Agregue también un **DbSet** a la clase `ProductsContext`, para que EF cree una tabla de clasificación en la base de datos.

[!code-csharp[Main](odata-actions-and-functions/samples/sample2.cs)]

### <a name="add-the-action-to-the-edm"></a>Agregar la acción al EDM

En WebApiConfig.cs, agregue el código siguiente:

[!code-csharp[Main](odata-actions-and-functions/samples/sample3.cs)]

El método **EntityTypeConfiguration. Action** agrega una acción a Entity Data Model (EDM). El método **Parameter** especifica un parámetro con tipo para la acción.

Este código también establece el espacio de nombres para el EDM. El espacio de nombres es importante porque el URI de la acción incluye el nombre completo de la acción:

[!code-console[Main](odata-actions-and-functions/samples/sample4.cmd)]

> [!NOTE]
> En una configuración típica de IIS, el punto de esta dirección URL hará que IIS devuelva el error 404. Puede resolver este paso agregando la siguiente sección al archivo Web. config:

[!code-xml[Main](odata-actions-and-functions/samples/sample5.xml)]

### <a name="add-a-controller-method-for-the-action"></a>Agregar un método de controlador para la acción

Para habilitar la &quot;frecuencia&quot; acción, agregue el método siguiente a `ProductsController`:

[!code-csharp[Main](odata-actions-and-functions/samples/sample6.cs)]

Observe que el nombre del método coincide con el nombre de la acción. El atributo **[HttpPost]** especifica que el método es un método post de http.

Para invocar la acción, el cliente envía una solicitud HTTP POST como la siguiente:

[!code-console[Main](odata-actions-and-functions/samples/sample7.cmd)]

La tasa de &quot;&quot; acción está enlazada a las instancias del producto, por lo que el URI de la acción es el nombre completo de la acción anexado al URI de la entidad. (Recuerde que establecemos el espacio de nombres EDM en &quot;ProductService&quot;, por lo que el nombre completo de la acción es &quot;ProductService. rate&quot;).

El cuerpo de la solicitud contiene los parámetros de acción como una carga JSON. Web API convierte automáticamente la carga de JSON en un objeto **ODataActionParameters** , que es simplemente un diccionario de valores de parámetro. Use este diccionario para tener acceso a los parámetros del método de controlador.

Si el cliente envía los parámetros de acción en el formato incorrecto, el valor de **ModelState. IsValid** es false. Active esta marca en el método de controlador y devuelva un error si **IsValid** es false.

[!code-csharp[Main](odata-actions-and-functions/samples/sample8.cs)]

## <a name="example-adding-a-function"></a>Ejemplo: agregar una función

Ahora vamos a agregar una función de OData que devuelve el producto más caro. Como antes, el primer paso es agregar la función al EDM. En WebApiConfig.cs, agregue el código siguiente.

[!code-csharp[Main](odata-actions-and-functions/samples/sample9.cs)]

En este caso, la función se enlaza a la colección Products, en lugar de a instancias de producto individuales. Los clientes invocan la función mediante el envío de una solicitud GET:

[!code-console[Main](odata-actions-and-functions/samples/sample10.cmd)]

Este es el método de controlador para esta función:

[!code-csharp[Main](odata-actions-and-functions/samples/sample11.cs)]

Observe que el nombre del método coincide con el nombre de la función. El atributo **[HttpGet]** especifica que el método es un método get de http.

Esta es la respuesta HTTP:

[!code-console[Main](odata-actions-and-functions/samples/sample12.cmd)]

## <a name="example-adding-an-unbound-function"></a>Ejemplo: agregar una función sin enlazar

El ejemplo anterior era una función enlazada a una colección. En el siguiente ejemplo, vamos a crear una función *sin enlazar* . Se llama a las funciones sin enlazar como operaciones estáticas en el servicio. La función de este ejemplo devolverá el impuesto de ventas para un código postal determinado.

En el archivo WebApiConfig, agregue la función al EDM:

[!code-csharp[Main](odata-actions-and-functions/samples/sample13.cs)]

Observe que se llama a la **función** directamente en **ODataModelBuilder**, en lugar del tipo de entidad o de la colección. Esto indica al generador de modelos que la función está desenlazada.

Este es el método de controlador que implementa la función:

[!code-csharp[Main](odata-actions-and-functions/samples/sample14.cs)]

No importa en qué controlador de API Web se coloca este método. Puede colocarlo en `ProductsController`o definir un controlador independiente. El atributo **[ODataRoute]** define la plantilla de URI para la función.

Este es un ejemplo de solicitud de cliente:

[!code-console[Main](odata-actions-and-functions/samples/sample15.cmd)]

La respuesta HTTP:

[!code-console[Main](odata-actions-and-functions/samples/sample16.cmd)]
