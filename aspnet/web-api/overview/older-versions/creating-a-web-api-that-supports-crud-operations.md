---
uid: web-api/overview/older-versions/creating-a-web-api-that-supports-crud-operations
title: Habilitar las operaciones CRUD en ASP.NET Web API 1-ASP.NET 4. x
author: MikeWasson
description: En el tutorial se muestra cómo admitir las operaciones CRUD en un servicio HTTP mediante ASP.NET Web API para ASP.NET 4. x.
ms.author: riande
ms.date: 01/28/2012
ms.custom: seoapril2019
ms.assetid: c125ca47-606a-4d6f-a1fc-1fc62928af93
msc.legacyurl: /web-api/overview/older-versions/creating-a-web-api-that-supports-crud-operations
msc.type: authoredcontent
ms.openlocfilehash: a096fd1c54df33b40115907a5c2517b2e3fec5b8
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/28/2019
ms.locfileid: "74600337"
---
# <a name="enabling-crud-operations-in-aspnet-web-api-1"></a>Habilitar operaciones CRUD en ASP.NET Web API 1

por [Mike Wasson](https://github.com/MikeWasson)

[Descargar proyecto completado](https://code.msdn.microsoft.com/ASP-NET-Web-API-Tutorial-c4761894)

> En este tutorial se muestra cómo admitir las operaciones CRUD en un servicio HTTP mediante ASP.NET Web API para ASP.NET 4. x.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versiones de software usadas en el tutorial
> 
> 
> - Visual Studio 2012
> - API Web 1 (también funciona con Web API 2)

CRUD significa &quot;crear, leer, actualizar y eliminar&quot; que son las cuatro operaciones básicas de base de datos. Muchos servicios HTTP también modelan las operaciones CRUD a través de las API de REST o del tipo REST.

En este tutorial, creará una API Web muy sencilla para administrar una lista de productos. Cada producto contendrá un nombre, un precio y una categoría (como &quot;juguetes&quot; o &quot;&quot;de hardware), además de un identificador de producto.

La API Products expondrá los siguientes métodos.

| Acción de | Método HTTP | URI relativo |
| --- | --- | --- |
| Obtener una lista de todos los productos | GET | /api/products |
| Obtener un producto por identificador | GET | *identificador* de/API/Products/ |
| Obtener un producto por categoría | GET | /API/Products? categoría =*categoría* |
| Crear un nuevo producto | Exponer | /api/products |
| Actualizar un producto | PUT | *identificador* de/API/Products/ |
| Eliminar un producto | SUPRIMIR | *identificador* de/API/Products/ |

Observe que algunos de los URI incluyen el ID. de producto en la ruta de acceso. Por ejemplo, para obtener el producto cuyo identificador es 28, el cliente envía una solicitud GET para `http://hostname/api/products/28`.

### <a name="resources"></a>Recursos

La API Products define los URI para dos tipos de recursos:

| Recurso | URI |
| --- | --- |
| Lista de todos los productos. | /api/products |
| Un producto individual. | *identificador* de/API/Products/ |

### <a name="methods"></a>Métodos

Los cuatro métodos HTTP principales (GET, PUT, POST y DELETE) pueden asignarse a las operaciones CRUD como se indica a continuación:

- GET recupera la representación del recurso en un URI especificado. GET no debe tener efectos secundarios en el servidor.
- PUT actualiza un recurso en un URI especificado. PUT también se puede usar para crear un nuevo recurso en un URI especificado, si el servidor permite a los clientes especificar nuevos URI. En este tutorial, la API no admitirá la creación a través de PUT.
- POST crea un nuevo recurso. El servidor asigna el URI para el nuevo objeto y devuelve este URI como parte del mensaje de respuesta.
- DELETE elimina un recurso en un URI especificado.

Nota: el método PUT reemplaza toda la entidad product. Es decir, se espera que el cliente envíe una representación completa del producto actualizado. Si desea admitir actualizaciones parciales, se prefiere el método PATCH. En este tutorial no se implementa PATCH.

## <a name="create-a-new-web-api-project"></a>Creación de un nuevo proyecto de API Web

Para empezar, ejecute Visual Studio y seleccione **nuevo proyecto** en la página de **Inicio** . O bien, en el menú **archivo** , seleccione **nuevo** y luego **proyecto**.

En el panel **plantillas** , seleccione **plantillas instaladas** y expanda el nodo **Visual C#**  . En **Visual C#** , seleccione **Web**. En la lista de plantillas de proyecto, seleccione **aplicación Web de ASP.NET MVC 4**. Asigne al proyecto el nombre &quot;ProductStore&quot; y haga clic en **Aceptar**.

![](creating-a-web-api-that-supports-crud-operations/_static/image1.png)

En el cuadro de diálogo **nuevo ASP.NET MVC 4** , seleccione **Web API** y haga clic en **Aceptar**.

![](creating-a-web-api-that-supports-crud-operations/_static/image2.png)

## <a name="adding-a-model"></a>Agregar un modelo

Un *modelo* es un objeto que representa los datos de la aplicación. En ASP.NET Web API, puede usar objetos CLR fuertemente tipados como modelos y se serializarán automáticamente a XML o JSON para el cliente.

En el caso de la API de ProductStore, nuestros datos se componen de productos, por lo que vamos a crear una nueva clase llamada `Product`.

Si Explorador de soluciones todavía no está visible, haga clic en el menú **Ver** y seleccione **Explorador de soluciones**. En Explorador de soluciones, haga clic con el botón secundario en la carpeta **modelos** . En el menú contextual, seleccione **Agregar**y, a continuación, seleccione **clase**. Asigne a la clase el nombre &quot;&quot;del producto.

![](creating-a-web-api-that-supports-crud-operations/_static/image3.png)

Agregue las siguientes propiedades a la clase `Product`.

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample1.cs)]

## <a name="adding-a-repository"></a>Agregar un repositorio

Necesitamos almacenar una colección de productos. Es una buena idea separar la colección de nuestra implementación del servicio. De este modo, podemos cambiar la memoria auxiliar sin volver a escribir la clase de servicio. Este tipo de diseño se denomina patrón de *repositorio* . Empiece por definir una interfaz genérica para el repositorio.

En Explorador de soluciones, haga clic con el botón secundario en la carpeta **modelos** . Seleccione **Agregar**y, a continuación, seleccione **nuevo elemento**.

![](creating-a-web-api-that-supports-crud-operations/_static/image4.png)

En el panel **plantillas** , seleccione **plantillas instaladas** y expanda el C# nodo. En C#, seleccione **código**. En la lista de plantillas de código, seleccione **interfaz**. Asigne a la interfaz el nombre &quot;IProductRepository&quot;.

![](creating-a-web-api-that-supports-crud-operations/_static/image5.png)

Agregue la siguiente implementación:

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample2.cs)]

Ahora agregue otra clase a la carpeta models, denominada &quot;ProductRepository.&quot; esta clase implementará la interfaz `IProductRepository`. Agregue la siguiente implementación:

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample3.cs)]

El repositorio mantiene la lista en la memoria local. Esto es correcto para un tutorial, pero en una aplicación real, almacenaría los datos externamente, ya sea una base de datos o un almacenamiento en la nube. El patrón de repositorio facilitará el cambio de la implementación más adelante.

## <a name="adding-a-web-api-controller"></a>Adición de un controlador de API Web

Si ha trabajado con ASP.NET MVC, ya está familiarizado con los controladores. En ASP.NET Web API, un *controlador* es una clase que controla las solicitudes HTTP desde el cliente. El Asistente para nuevo proyecto creó dos controladores automáticamente al crear el proyecto. Para verlos, expanda la carpeta Controllers en Explorador de soluciones.

- HomeController es un controlador ASP.NET MVC tradicional. Es responsable de proporcionar páginas HTML para el sitio y no está directamente relacionado con nuestra API Web.
- Clase valuescontroller es un controlador de WebAPI de ejemplo.

Continúe y elimine clase valuescontroller; para ello, haga clic con el botón derecho en el archivo en Explorador de soluciones y seleccione **eliminar.** Ahora agregue un nuevo controlador, como se indica a continuación:

En **Explorador de soluciones**, haga clic con el botón secundario en la carpeta Controllers. Seleccione **Agregar** y, a continuación, seleccione **controlador**.

![](creating-a-web-api-that-supports-crud-operations/_static/image6.png)

En el Asistente para **Agregar controlador** , asigne al controlador el nombre &quot;ProductsController&quot;. En la lista desplegable **plantilla** , seleccione **controlador de API vacío**. A continuación, haga clic en **Agregar**.

![](creating-a-web-api-that-supports-crud-operations/_static/image7.png)

> [!NOTE]
> No es necesario colocar los controladores en una carpeta denominada Controllers. El nombre de la carpeta no es importante; es simplemente una manera cómoda de organizar los archivos de código fuente.

El Asistente para **agregar controladores** creará un archivo denominado ProductsController.CS en la carpeta Controllers. Si este archivo no está abierto, haga doble clic en el archivo para abrirlo. Agregue la siguiente instrucción **using** :

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample4.cs)]

Agregue un campo que contenga una instancia de **IProductRepository** .

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample5.cs)]

> [!NOTE]
> Llamar a `new ProductRepository()` en el controlador no es el mejor diseño, ya que une el controlador a una implementación determinada de `IProductRepository`. Para obtener un mejor enfoque, consulte [uso de la resolución de dependencia de la API Web](../advanced/dependency-injection.md).

## <a name="getting-a-resource"></a>Obtención de un recurso

La API de ProductStore expondrá varios &quot;leer&quot; acciones como métodos GET de HTTP. Cada acción se corresponderá con un método de la clase `ProductsController`.

| Acción de | Método HTTP | URI relativo |
| --- | --- | --- |
| Obtener una lista de todos los productos | GET | /api/products |
| Obtener un producto por identificador | GET | *identificador* de/API/Products/ |
| Obtener un producto por categoría | GET | /API/Products? categoría =*categoría* |

Para obtener la lista de todos los productos, agregue este método a la clase `ProductsController`:

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample6.cs)]

El nombre del método comienza con &quot;obtener&quot;, por lo que, por Convención, se asigna a las solicitudes GET. Además, dado que el método no tiene parámetros, se asigna a un URI que no contiene un *identificador de&quot;&quot;* segmento de la ruta de acceso.

Para obtener un producto por identificador, agregue este método a la clase `ProductsController`:

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample7.cs)]

Este nombre de método también empieza por &quot;obtener&quot;, pero el método tiene un parámetro denominado *ID*. Este parámetro se asigna al &quot;ID&quot; segmento de la ruta de acceso del identificador URI. El marco de trabajo de ASP.NET Web API convierte automáticamente el identificador en el tipo de datos correcto (**int**) del parámetro.

El método GetProduct produce una excepción de tipo **HttpResponseException** si el *identificador* no es válido. El marco de trabajo traducirá esta excepción a un error 404 (no encontrado).

Por último, agregue un método para buscar productos por Categoría:

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample8.cs)]

Si el URI de solicitud tiene una cadena de consulta, la API Web intenta coincidir los parámetros de consulta con los parámetros del método de controlador. Por lo tanto, un URI con el formato "API/Products? Category =*Category*" se asignará a este método.

## <a name="creating-a-resource"></a>Creación de un recurso

A continuación, agregaremos un método a la clase `ProductsController` para crear un nuevo producto. A continuación se muestra una implementación sencilla del método:

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample9.cs)]

Tenga en cuenta dos cosas sobre este método:

- El nombre del método comienza con &quot;post...&quot;. Para crear un nuevo producto, el cliente envía una solicitud HTTP POST.
- El método toma un parámetro de tipo product. En Web API, los parámetros con tipos complejos se deserializan del cuerpo de la solicitud. Por lo tanto, esperamos que el cliente envíe una representación serializada de un objeto product, en formato XML o JSON.

Esta implementación funcionará, pero no es bastante completa. Idealmente, nos gustaría que la respuesta HTTP incluira lo siguiente:

- **Código de respuesta:** De forma predeterminada, el marco de la API Web establece el código de estado de la respuesta en 200 (correcto). Pero según el protocolo HTTP/1.1, cuando una solicitud POST tiene como resultado la creación de un recurso, el servidor debe responder con el estado 201 (creado).
- **Ubicación:** Cuando el servidor crea un recurso, debe incluir el URI del nuevo recurso en el encabezado de ubicación de la respuesta.

ASP.NET Web API facilita la manipulación del mensaje de respuesta HTTP. Esta es la implementación mejorada:

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample10.cs)]

Observe que el tipo de valor devuelto del método es ahora **HttpResponseMessage**. Al devolver un **HttpResponseMessage** en lugar de un producto, se pueden controlar los detalles del mensaje de respuesta http, incluido el código de estado y el encabezado de ubicación.

El método **CreateResponse** crea una **HttpResponseMessage** y escribe automáticamente una representación serializada del objeto Product en el cuerpo del mensaje de respuesta.

> [!NOTE]
> En este ejemplo no se valida el `Product`. Para obtener información sobre la validación de modelos, vea [validación de modelos en ASP.net web API](../formats-and-model-binding/model-validation-in-aspnet-web-api.md).

## <a name="updating-a-resource"></a>Actualización de un recurso

Actualizar un producto con PUT es sencillo:

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample11.cs)]

El nombre del método comienza con &quot;Put...&quot;, por lo que la API Web lo hace coincidir con las solicitudes PUT. El método toma dos parámetros, el identificador de producto y el producto actualizado. El parámetro *ID* se toma de la ruta de acceso del URI y el parámetro *Product* se deserializa del cuerpo de la solicitud. De forma predeterminada, el marco de ASP.NET Web API toma tipos de parámetros simples de la ruta y los tipos complejos del cuerpo de la solicitud.

## <a name="deleting-a-resource"></a>Eliminar un recurso

Para eliminar un recurso, defina una "eliminar..." forma.

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample12.cs)]

Si una solicitud de eliminación se realiza correctamente, puede devolver el estado 200 (correcto) con un cuerpo de entidad que describe el estado. Estado 202 (aceptado) si la eliminación todavía está pendiente; o el estado 204 (sin contenido) sin cuerpo de entidad. En este caso, el método `DeleteProduct` tiene un tipo de valor devuelto `void`, por lo que ASP.NET Web API traduce automáticamente en el código de estado 204 (sin contenido).
