---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-6
title: 'Parte 6: creación de controladores de productos y pedidos | Microsoft Docs'
author: MikeWasson
description: ''
ms.author: riande
ms.date: 07/04/2012
ms.assetid: 91ee29ee-0689-40ee-914a-e7dd733b6622
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-6
msc.type: authoredcontent
ms.openlocfilehash: e0bf88e3477acbde910cde956042449bc86ce79a
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/28/2019
ms.locfileid: "74600025"
---
# <a name="part-6-creating-product-and-order-controllers"></a>Parte 6: creación de controladores de productos y pedidos

por [Mike Wasson](https://github.com/MikeWasson)

[Descargar proyecto completado](https://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="add-a-products-controller"></a>Agregar un controlador de productos

El controlador de administración es para los usuarios que tienen privilegios de administrador. Por otra parte, los clientes pueden ver los productos pero no pueden crearlos, actualizarlos ni eliminarlos.

Se puede restringir fácilmente el acceso a los métodos post, Put y DELETE, a la vez que se dejan los métodos Get abiertos. Pero fíjese en los datos que se devuelven para un producto:

[!code-json[Main](using-web-api-with-entity-framework-part-6/samples/sample1.json?highlight=1)]

La propiedad `ActualCost` no debe estar visible para los clientes. La solución consiste en definir un *objeto de transferencia de datos* (DTO) que incluya un subconjunto de propiedades que deben ser visibles para los clientes. Usaremos LINQ to Project `Product` instancias para `ProductDTO` instancias.

Agregue una clase denominada `ProductDTO` a la carpeta models.

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample2.cs)]

Ahora, agregue el controlador. En Explorador de soluciones, haga clic con el botón secundario en la carpeta Controllers. Seleccione **Agregar**y, a continuación, seleccione **controlador**. En el cuadro de diálogo **Agregar controlador** , asigne al controlador el nombre &quot;ProductsController&quot;. En **plantilla**, seleccione **controlador de API vacío**.

![](using-web-api-with-entity-framework-part-6/_static/image1.png)

Reemplace todo en el archivo de código fuente por el código siguiente:

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample3.cs)]

El controlador sigue utilizando el `OrdersContext` para consultar la base de datos. Pero en lugar de devolver instancias de `Product` directamente, llamamos a `MapProducts` para proyectarlas en `ProductDTO` instancias:

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample4.cs?highlight=1)]

El método `MapProducts` devuelve un **IQueryable**, por lo que podemos crear el resultado con otros parámetros de consulta. Puede verlo en el método `GetProduct`, que agrega una cláusula **Where** a la consulta:

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample5.cs?highlight=2)]

## <a name="add-an-orders-controller"></a>Agregar un controlador de pedidos

A continuación, agregue un controlador que permita a los usuarios crear y ver los pedidos.

Comenzaremos con otro DTO. En Explorador de soluciones, haga clic con el botón secundario en la carpeta modelos y agregue una clase denominada `OrderDTO` use la siguiente implementación:

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample6.cs)]

Ahora, agregue el controlador. En Explorador de soluciones, haga clic con el botón secundario en la carpeta Controllers. Seleccione **Agregar**y, a continuación, seleccione **controlador**. En el cuadro de diálogo **Agregar controlador** , establezca las siguientes opciones:

- En **nombre del controlador**, escriba "OrdersController".
- En **plantilla**, seleccione "controlador de API con acciones de lectura y escritura, con Entity Framework".
- En **clase de modelo**, seleccione &quot;orden (ProductStore. Models)&quot;.
- En **clase de contexto de datos**, seleccione &quot;OrdersContext (ProductStore. Models)&quot;.

![](using-web-api-with-entity-framework-part-6/_static/image2.png)

Haga clic en **Agregar**. Esto agrega un archivo denominado OrdersController.cs. A continuación, es necesario modificar la implementación predeterminada del controlador.

En primer lugar, elimine los métodos `PutOrder` y `DeleteOrder`. En este ejemplo, los clientes no pueden modificar ni eliminar los pedidos existentes. En una aplicación real, se necesitaría una gran cantidad de lógica de back-end para controlar estos casos. (Por ejemplo, ¿el pedido ya se ha enviado?)

Cambie el método `GetOrders` para devolver solo los pedidos que pertenecen al usuario:

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample7.cs)]

Cambie el método `GetOrder` como se indica a continuación:

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample8.cs)]

Estos son los cambios que hemos realizado en el método:

- El valor devuelto es una instancia de `OrderDTO`, en lugar de una `Order`.
- Cuando se consulta la base de datos para el pedido, usamos el método [DbQuery. include](https://msdn.microsoft.com/library/gg696395) para capturar las entidades `OrderDetail` y `Product` relacionadas.
- El resultado se acopla mediante una proyección.

La respuesta HTTP contendrá una matriz de productos con cantidades:

[!code-json[Main](using-web-api-with-entity-framework-part-6/samples/sample9.json)]

Este formato es más fácil para que los clientes consuman el gráfico de objetos original, que contiene entidades anidadas (Order, details y Products).

Último método que se va a considerar `PostOrder`. En este momento, este método toma una instancia de `Order`. Pero tenga en cuenta lo que sucede si un cliente envía un cuerpo de solicitud de la siguiente manera:

[!code-json[Main](using-web-api-with-entity-framework-part-6/samples/sample10.json)]

Este es un orden bien estructurado y Entity Framework se insertará en la base de datos. Pero contiene una entidad de producto que no existía anteriormente. El cliente acaba de crear un nuevo producto en nuestra base de datos. Esto supondrá una sorpresa en el Departamento de realización de pedidos, cuando vea un pedido de Koala. La moral es, en realidad, tenga cuidado con los datos que acepta en una solicitud POST o PUT.

Para evitar este problema, cambie el método `PostOrder` para tomar una instancia `OrderDTO`. Utilice la `OrderDTO` para crear el `Order`.

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample11.cs)]

Tenga en cuenta que se usan las propiedades `ProductID` y `Quantity`, y se omiten los valores que el cliente envió por nombre de producto o precio. Si el ID. de producto no es válido, infringirá la restricción FOREIGN KEY en la base de datos y se producirá un error en la inserción, como debería.

Este es el método de `PostOrder` completo:

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample12.cs)]

Por último, agregue el atributo **Authorize** al controlador:

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample13.cs)]

Ahora solo los usuarios registrados pueden crear o ver los pedidos.

> [!div class="step-by-step"]
> [Anterior](using-web-api-with-entity-framework-part-5.md)
> [Siguiente](using-web-api-with-entity-framework-part-7.md)
