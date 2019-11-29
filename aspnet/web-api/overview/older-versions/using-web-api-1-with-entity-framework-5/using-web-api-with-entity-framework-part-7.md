---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-7
title: 'Parte 7: crear la Página principal | Microsoft Docs'
author: MikeWasson
description: ''
ms.author: riande
ms.date: 07/04/2012
ms.assetid: eb32a17b-626c-4373-9a7d-3387992f3c04
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-7
msc.type: authoredcontent
ms.openlocfilehash: fe4074c701159a137be3644d65ca844f160c2399
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/28/2019
ms.locfileid: "74599986"
---
# <a name="part-7-creating-the-main-page"></a>Parte 7: creación de la Página principal

por [Mike Wasson](https://github.com/MikeWasson)

[Descargar proyecto completado](https://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="creating-the-main-page"></a>Crear la página principal

En esta sección, creará la Página principal de la aplicación. Esta página será más compleja que la página Administrador, por lo que nos centraremos en varios pasos. A lo largo del proceso, verá algunas técnicas más avanzadas de Knockout. js. Este es el diseño básico de la página:

![](using-web-api-with-entity-framework-part-7/_static/image1.png)

- "Productos" contiene una matriz de productos.
- "Cart" contiene una matriz de productos con cantidades. Al hacer clic en "agregar al carro", se actualiza el carro.
- "Orders" contiene una matriz de identificadores de pedido.
- "Detalles" contiene un detalle del pedido, que es una matriz de elementos (productos con cantidades)

Comenzaremos definiendo un diseño básico en HTML, sin enlace de datos ni scripts. Abra el archivo views/home/index. cshtml y reemplace todo el contenido por lo siguiente:

[!code-html[Main](using-web-api-with-entity-framework-part-7/samples/sample1.html)]

A continuación, agregue una sección scripts y cree un modelo de vista vacía:

[!code-cshtml[Main](using-web-api-with-entity-framework-part-7/samples/sample2.cshtml)]

Según el diseño esbozado anteriormente, nuestro modelo de vista necesita observables para productos, carros, pedidos y detalles. Agregue las siguientes variables al objeto `AppViewModel`:

[!code-javascript[Main](using-web-api-with-entity-framework-part-7/samples/sample3.js)]

Los usuarios pueden agregar elementos de la lista de productos al carro y quitar elementos del carro. Para encapsular estas funciones, vamos a crear otra clase de modelo de vista que representa un producto. Agrega el código siguiente a `AppViewModel`:

[!code-javascript[Main](using-web-api-with-entity-framework-part-7/samples/sample4.js?highlight=4)]

La clase `ProductViewModel` contiene dos funciones que se usan para trasladar el producto hacia y desde el carro: `addItemToCart` agrega una unidad del producto al carro y `removeAllFromCart` quita todas las cantidades del producto.

Los usuarios pueden seleccionar un pedido existente y obtener los detalles del pedido. Encapsularemos esta funcionalidad en otro modelo de vista:

[!code-javascript[Main](using-web-api-with-entity-framework-part-7/samples/sample5.js?highlight=4)]

El `OrderDetailsViewModel` se inicializa con un pedido y captura los detalles del pedido mediante el envío de una solicitud AJAX al servidor.

Observe también la propiedad `total` en el `OrderDetailsViewModel`. Esta propiedad es una clase especial de observable denominada [observable calculada](http://knockoutjs.com/documentation/computedObservables.html). Como implica el nombre, un objeto observable calculado permite enlazar datos a un valor&#8212;calculado en este caso, el costo total del pedido.

A continuación, agregue estas funciones a `AppViewModel`:

- `resetCart` quita todos los elementos del carro.
- `getDetails` obtiene los detalles de un pedido (insertando un nuevo `OrderDetailsViewModel` en la lista de `details`).
- `createOrder` crea un nuevo pedido y vacía el carro.

[!code-javascript[Main](using-web-api-with-entity-framework-part-7/samples/sample6.js?highlight=4)]

Por último, inicialice el modelo de vista realizando solicitudes AJAX para los productos y pedidos:

[!code-javascript[Main](using-web-api-with-entity-framework-part-7/samples/sample7.js)]

Bien, eso es una gran cantidad de código, pero lo compilamos paso a paso, por lo que espero que el diseño esté claro. Ahora podemos agregar algunos enlaces Knockout. js al código HTML.

**Productos**

Estos son los enlaces para la lista de productos:

[!code-html[Main](using-web-api-with-entity-framework-part-7/samples/sample8.html)]

Esto recorre en iteración la matriz de productos y muestra el nombre y el precio. El botón "Agregar a pedido" solo está visible cuando el usuario ha iniciado sesión.

El botón "Agregar a pedido" llama `addItemToCart` en la instancia de `ProductViewModel` para el producto. Esto muestra una interesante característica de Knockout. js: cuando un modelo de vista contiene otros modelos de vista, puede aplicar los enlaces al modelo interno. En este ejemplo, los enlaces dentro del `foreach` se aplican a cada una de las instancias de `ProductViewModel`. Este enfoque es mucho más limpio que poner toda la funcionalidad en un único modelo de vista.

**Maletín**

Estos son los enlaces para el carro:

[!code-html[Main](using-web-api-with-entity-framework-part-7/samples/sample9.html)]

Esto recorre en iteración la matriz del carro y muestra el nombre, el precio y la cantidad. Tenga en cuenta que el vínculo "quitar" y el botón "crear pedido" están enlazados a las funciones de modelo de vista.

**Pedidos**

Estos son los enlaces para la lista de pedidos:

[!code-html[Main](using-web-api-with-entity-framework-part-7/samples/sample10.html)]

Esto recorre en iteración los pedidos y muestra el ID. de pedido. El evento click del vínculo se enlaza a la función `getDetails`.

**Detalles del pedido**

Estos son los enlaces de los detalles del pedido:

[!code-html[Main](using-web-api-with-entity-framework-part-7/samples/sample11.html)]

Esto recorre en iteración los elementos en el orden y muestra el producto, el precio y la cantidad. El div circundante solo es visible si la matriz de detalles contiene uno o varios elementos.

## <a name="conclusion"></a>Conclusión

En este tutorial, ha creado una aplicación que usa Entity Framework para comunicarse con la base de datos y ASP.NET Web API para proporcionar una interfaz de acceso público en la parte superior de la capa de datos. Usamos ASP.NET MVC 4 para presentar las páginas HTML y Knockout. js más jQuery para proporcionar interacciones dinámicas sin recargas de páginas.

Recursos adicionales:

- [Mapa de contenido de ASP.NET Data Access](https://msdn.microsoft.com/library/6759sth4.aspx)
- [Centro para desarrolladores de Entity Framework](https://msdn.microsoft.com/data/ef)

> [!div class="step-by-step"]
> [Anterior](using-web-api-with-entity-framework-part-6.md)
